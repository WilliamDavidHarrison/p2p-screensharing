<script setup lang="ts">
import { DataConnection } from "peerjs";
import { computed, onMounted, onUnmounted, ref, watchEffect } from "vue";
import { useRouter } from "vue-router";
import Brand from "../components/Brand.vue";
import Button from "../components/Button.vue";
import Modal from "../components/Modal.vue";
import Navbar from "../components/Navbar.vue";
import UsernameModal from "../components/UsernameModal.vue";
import WaitingModal from "../components/WaitingModal.vue";
import { connected, connectionToPeer, peer, peerId, peerName } from "../lib/peer";
import { username } from "../lib/user";

const router = useRouter();

const url = computed(() => {
    return `${location.protocol}//${location.host}/join?id=${peerId.value}`;
});

const copied = ref(false);

function copyToClipboard() {
    if(copied.value) return;

    navigator.clipboard.writeText(url.value).then(() => {
        copied.value = true;

        setTimeout(() => {
            copied.value = false;
        }, 1000);
    });
}

// Requests
interface Request {
    username: string;
    connection: DataConnection;
}

const queue = ref<Request[]>([]);

onMounted(() => {
    peer.on("connection", (connection) => {
        connection.once("data", (data) => {
            if(typeof data !== "string") return connection.close();
            if(data === username.value) return connection.close();

            queue.value.push({
                connection,
                username: data,
            });
        });

        connection.on("close", () => {
            queue.value = queue.value.filter((item) => {
                return item.connection.peer !== connection.peer;
            });
        });
    });
});

onUnmounted(() => {
    // Close pending connections
    queue.value.forEach((request) => {
        request.connection.close();
    });

    peer.off("connection");
});

function decline(request: Request) {
    request.connection.close();
}

function accept(request: Request) {
    request.connection.send(username.value);
    request.connection.off("close");

    peerName.value = request.username;
    connectionToPeer.value = request.connection;
    // Remove from requests so it is not closed during cleanup
    queue.value = queue.value.filter((item) => {
        return item.connection.peer !== request.connection.peer;
    });
}

watchEffect(() => {
    if(peerName.value && connectionToPeer.value) router.push(`/rooms/${peerId.value}`);
});
</script>

<template>
    <Navbar>
        <Brand />
    </Navbar>

    <Modal :show="!username.length">
        <UsernameModal />
    </Modal>

    <Modal :show="username.length > 0 && !connected">
        <WaitingModal title="Creating room..." @cancel="router.push('/')" />
    </Modal>

    <main
        v-if="username.length && connected"
        class="flex-1 flex flex-col space-y-6 items-center justify-center px-6 text-center mx-auto"
    >
        <h1 class="text-3xl font-bold">🎉 Your room has been created!</h1>

        <p class="text-gray-400 text-lg font-semibold">
            Your username is <span class="text-blue-400">{{ username }}</span>.
            <br>
            <span class="font-normal text-xs">Any requests using the same username will be automatically rejected.</span>
        </p>

        <p class="text-blue-400 font-semibold">
            {{ url }}
            <br>
            <span class="font-normal text-xs text-gray-400">Share this link with someone to invite them to your room.</span>
        </p>

        <Button type="button" class="w-30" @click="copyToClipboard">
            {{ copied ? "Copied!" : "Copy Link" }}
        </Button>
    </main>

    <Modal :show="queue.length > 0">
        <div v-if="queue.length" class="text-center">
            <p class="text-lg mt-2 mb-6"><strong>{{ queue[0].username }}</strong> wants to join your room.</p>

            <div class="space-x-2">
                <Button type="button" class="bg-green-500 hover:bg-green-600 flex-1" @click="accept(queue[0] as Request)">Accept</Button>
                <Button type="button" class="bg-red-500 hover:bg-red-600 flex-1" @click="decline(queue[0] as Request)">Decline</Button>
            </div>
        </div>
    </Modal>
</template>
