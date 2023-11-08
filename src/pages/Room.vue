<script setup lang="ts">
import { computed, onMounted, onUnmounted, ref } from "vue";
import { useRoute, useRouter } from "vue-router";
import Button from "../components/Button.vue";
import Modal from "../components/Modal.vue";
import Navbar from "../components/Navbar.vue";
import Stream from "../components/Stream.vue";
import Chat from "../components/Chat.vue";
import { connectionToPeer, peer, peerId, peerName } from "../lib/peer";

const route = useRoute();

const id = computed(() => {
    if(Array.isArray(route.params.id)) {
        return route.params.id[0];
    }

    return route.params.id;
});

const isHost = computed(() => id.value === peerId.value);

// Ping to detect broken connections
const lastPing = ref(Date.now());
const interval = setInterval(() => {
    connectionToPeer.value?.send("ping");

    if(lastPing.value < Date.now() - 7000) {
        connectionToPeer.value?.close();
    }
}, 5000);

onMounted(() => {
    // Update ping value
    connectionToPeer.value?.on("data", (data) => {
        if(data === "ping") {
            lastPing.value = Date.now();
        }
    });
});

onUnmounted(() => {
    clearInterval(interval);
    connectionToPeer.value?.off("data");
});

// Reject other conenctions and calls
onMounted(() => {
    peer.on("connection", (connection) => { connection.close(); });

    peer.on("call", (mediaConnection) => {
        if(mediaConnection.peer !== connectionToPeer.value?.peer) mediaConnection.close();
    });
});

// Clear up local events and global store
onUnmounted(() => {
    peer.off("connection");
    peer.off("call");
    peerName.value = undefined;
    connectionToPeer.value?.close();
    connectionToPeer.value = undefined;
});

// Peer has left
const router = useRouter();
const roomClosed = ref(false);

onMounted(() => {
    connectionToPeer.value?.on("close", () => { roomClosed.value = true; });
});
</script>

<template>
    <Navbar>
        <h1 class="font-semibold">{{ isHost ? "Your" : `${peerName}'s` }} Room</h1>

        <button type="button" class="text-red-400 hover:text-red-500 font-semibold transition-colors" @click="router.push('/')">
            {{ isHost ? "Close Room" : "Leave Room" }}
        </button>
    </Navbar>

    <main class="flex-1 flex flex-col lg:flex-row">
        <div class="lg:w-[70vw] max-w-[1350px] lg:border-r lg:border-r-slate-700">
            <Stream :is-host="isHost" />
        </div>

        <div class="flex-1">
            <Chat />
        </div>
    </main>

    <Modal :show="roomClosed">
        <div class="text-center">
            <h1 class="text-xl mt-2 mb-4"><strong>{{ peerName }}</strong> has {{ isHost ? "left" : "closed" }} the room.</h1>

            <Button class="bg-red-500 hover:bg-red-400 transition-colors" @click="router.push('/')">Go Home</Button>
        </div>
    </Modal>
</template>
