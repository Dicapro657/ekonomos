<template>
  <div style="background: #000; color: #fff; min-height: 100vh; display: flex; flex-direction: column; align-items: center; justify-content: center;">
    <h1>EKONOMOS</h1>
    <p>Quantum-grade Web3 Finance</p>
    <button @click="connectWallet">Connect MetaMask</button>
    <input v-model="amount" type="number" placeholder="Enter donation amount (ETH)" />
    <button @click="sendDonation">Support</button>
  </div>
</template>

<script setup>
import { ref } from 'vue'
import { ethers } from 'ethers'

const amount = ref('')

const connectWallet = async () => {
  if (typeof window.ethereum !== 'undefined') {
    try {
      await window.ethereum.request({ method: 'eth_requestAccounts' })
      alert('MetaMask connected')
    } catch (err) {
      console.error('Connection error', err)
    }
  } else {
    alert('MetaMask not detected')
  }
}

const sendDonation = async () => {
  if (!amount.value) return alert('Enter amount')
  const provider = new ethers.BrowserProvider(window.ethereum)
  const signer = await provider.getSigner()
  const tx = await signer.sendTransaction({
    to: '0x000000000000000000000000000000000000dead',
    value: ethers.parseEther(amount.value)
  })
  await tx.wait()
  alert('Thank you for supporting!')
}
</script>