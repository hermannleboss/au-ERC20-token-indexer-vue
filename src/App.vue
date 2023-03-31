<script setup lang="ts">
import {onMounted, ref} from "vue";
import {Alchemy, Network, Utils} from 'alchemy-sdk';

const config = {
    apiKey: import.meta.env.VITE_API_KEY,
    network: Network.ETH_MAINNET,
};
const alchemy = new Alchemy(config);
const text = ref("")

const isWalletConnected = async () => {
    let status = false
    try {
        const {ethereum} = window;

        const accounts = await ethereum.request({method: 'eth_accounts'})

        if (accounts.length > 0) {
            const account = accounts[0];
            console.log("wallet is connected! " + account);
            text.value = accounts.pop()
            status = true
        } else {
            console.log("make sure MetaMask is connected");
        }
    } catch (error) {
        console.log("error: ", error);
    }
    return status
}

async function connectWallet() {
    try {
        if (typeof window.ethereum !== 'undefined') {
            console.log('MetaMask is installed!');
        }
        const {ethereum} = window;

        if (!ethereum) {
            console.log("please install MetaMask");
        }

        const accounts = await ethereum.request({
            method: 'eth_requestAccounts'
        });
        text.value = accounts.pop()

    } catch (error) {
        console.log(error);
    }
}

onMounted(() => {
    isWalletConnected()
})

function checkToken() {

}
</script>

<template>
    <header class="container mx-auto  my-4 p-4">
        <h1 class="text-2xl text-center font-bold">ERC-20 Token Indexer</h1>
    </header>

    <main class="container mx-auto bg-white my-4 p-4 rounded border text-center flex flex-col items-center">
        <p class="">Plug in an address and this website will return all of its ERC-20 token balances!</p>
        <h2 class="text-xl font-bold py-3">Get all the ERC-20 token balances of this address:</h2>
        <button class="bg-gray-100 border p-3 m-2" @click="connectWallet()" >Connect your wallet</button>
        <p>Or</p>
        <input
                :value="text"
                @input="event => text = event.target.value"
                placeholder="Type And address"
                class="mt-1 inline-block w-[25%] px-3 py-2 bg-white border border-slate-300 rounded-md text-sm shadow-sm placeholder-slate-400
      focus:outline-none focus:border-sky-500 focus:ring-1 focus:ring-sky-500
      disabled:bg-slate-50 disabled:text-slate-500 disabled:border-slate-200 disabled:shadow-none
      invalid:border-pink-500 invalid:text-pink-600
      focus:invalid:border-pink-500 focus:invalid:ring-pink-500
    ">
        <button class="bg-blue-600 text-white rounded-lg border p-3 m-4">Check ERC-20 Token Balances</button>

        <h2 class="text-xl font-bold py-3">ERC-20 token balances:</h2>
        <div class="flex gap-5 flex-wrap">
            <div class="border bg-white w-[20vw] min-h-[250px]">
                <p>Symbol: ETH</p>
                <p>Balance: 15</p>
                <img src="https://placehold.co/600x400" alt="Placeholder">
            </div>
            <div class="border bg-white w-[20vw] min-h-[250px]">
                <p>Symbol: ETH</p>
                <p>Balance: 15</p>
                <img src="https://placehold.co/600x400" alt="Placeholder">
            </div>
            <div class="border bg-white min-w-[150px] min-h-[250px]">
            </div>
            <div class="border bg-white min-w-[150px] min-h-[250px]">
            </div>
            <div class="border bg-white min-w-[150px] min-h-[250px]">
            </div>
        </div>
    </main>
</template>

<style scoped>
</style>
