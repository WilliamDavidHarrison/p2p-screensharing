<script setup lang="ts">
import { useRouter } from "vue-router";
import Brand from "../components/Brand.vue";
import Button from "../components/Button.vue";
import Navbar from "../components/Navbar.vue";

const router = useRouter();

function github() {
    window.open("https://github.com/WilliamDavidHarrison/p2p-screensharing");
}
</script>

<template>
    <Navbar>
        <Brand />
        <Button type="button" class="bg-gray-500 hover:bg-gray-600" @click="github()">GitHub</Button>
    </Navbar>

    <main className="flex-1 flex flex-col items-center justify-center space-y-10 text-center">
        <h1 className="text-2xl md:text-3xl font-bold">p2p Screensharing</h1>
        <h2 className="max-w-xs text-lg md:text-xl md:max-w-md">
            Chat and screenshare peer-to-peer, without a server, no account needed.
        </h2>
        <Button type="button" class="text-lg shadow-lg" @click="router.push('/create')"> Create Room </Button>
    </main>
</template>
