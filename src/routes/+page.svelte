<script lang="ts">
  import { onMount } from 'svelte';
  import { Connection, PublicKey } from '@solana/web3.js';
  import { Program, AnchorProvider, web3 } from '@project-serum/anchor';
  import idl from '$lib/idl.json';

  let cropName = '';
  let proposals = [];
  let walletPublicKey = '';
  let program;

  const network = 'https://api.devnet.solana.com';
  const programID = new PublicKey(idl.metadata.address);

  function getProvider() {
    const connection = new Connection(network, 'processed');
    const wallet = window.solana;
    return new AnchorProvider(connection, wallet, { preflightCommitment: 'processed' });
  }

  async function connectWallet() {
    const resp = await window.solana.connect();
    walletPublicKey = resp.publicKey.toString();
    fetchProgram();
  }

  async function fetchProgram() {
    const provider = getProvider();
    program = new Program(idl, programID, provider);
    fetchProposals();
  }

  async function fetchProposals() {
    try {
      proposals = await program.account.cropProposal.all();
    } catch (err) {
      console.error('Error fetching proposals:', err);
    }
  }

  async function submitProposal() {
    const provider = getProvider();
    const [proposalPda] = await PublicKey.findProgramAddress([
      Buffer.from('proposal'),
      new PublicKey(walletPublicKey).toBuffer()
    ], programID);

    await program.methods.createProposal(cropName)
      .accounts({
        proposal: proposalPda,
        user: new PublicKey(walletPublicKey),
        systemProgram: web3.SystemProgram.programId
      }).rpc();

    cropName = '';
    fetchProposals();
  }

  async function voteOnProposal(proposalPubkey) {
    await program.methods.vote()
      .accounts({
        proposal: new PublicKey(proposalPubkey),
        user: new PublicKey(walletPublicKey)
      }).rpc();
    fetchProposals();
  }

  onMount(() => {
    if (window.solana && window.solana.isPhantom) {
      connectWallet();
    }
  });
</script>

<style>
  input, button {
    margin: 0.5rem;
    padding: 0.5rem;
  }
</style>

<h1>🌿 Agri-Tech DAO Marketplace</h1>

<input placeholder="Enter crop name" bind:value={cropName} />
<button on:click={submitProposal}>Submit Proposal</button>

<h2>🕳 DAO Crop Proposals</h2>
<table>
  <thead>
    <tr>
      <th>Crop</th>
      <th>Votes</th>
      <th>Action</th>
    </tr>
  </thead>
  <tbody>
    {#each proposals as item}
      <tr>
        <td>{item.account.name}</td>
        <td>{item.account.votes.toString()}</td>
        <td><button on:click={() => voteOnProposal(item.publicKey)}>Vote</button></td>
      </tr>
    {/each}
  </tbody>
</table>
