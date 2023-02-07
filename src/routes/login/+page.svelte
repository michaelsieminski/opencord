<script>
	import { onMount } from 'svelte';
	import { fade } from 'svelte/transition';
	import { generatePrivateKey, getPublicKey } from 'nostr-tools';

	import Logo from '$lib/logo.svelte';

	let error = '';
	let privkey = null;
	let hasExtension = false;

	onMount(() => {
		setTimeout(() => {
			hasExtension = !!window.nostr;
		}, 1000);
	});

	const generatePrivKey = () => {
		privkey = generatePrivateKey();
	};

	const logInWithExtension = async () => {
		const pubkey = await window.nostr.getPublicKey();

		if (!pubkey.match(/[a-z0-9{64}]/)) {
			error = 'The entered public key is invalid. Please try again.';
		} else {
			privkey = '';
		}
	};
</script>

<div class="w-full max-w-3xl mx-auto mt-20 text-center">
	<h1 class="mb-4 text-4xl font-bold tracking-tight uppercase">Welcome to</h1>
	<div class="h-16 mt-14">
		<Logo />
	</div>

	<div class="w-full h-full p-12 mt-20 text-left bg-white border shadow-lg rounded-xl">
		<h2 class="mb-6 text-2xl font-bold tracking-tight text-center">Log in</h2>

		<p class="mb-6 text-neutral-700">
			Please enter the private key for your Nostr Account below or use a <a
				href="https://github.com/nostr-protocol/nips/blob/master/07.md"
				target="_blank"
				rel="noopener noreferrer"
				class="transition text-emerald-500 hover:text-opencord">compatible browser extension</a
			> to log in.
		</p>

		<div class="flex flex-col mt-1">
			<label for="privkey" class="sr-only">Private Key</label>
			<div class="relative">
				<input
					bind:value={privkey}
					name="privkey"
					id="privkey"
					type="text"
					placeholder="Enter your Private Key..."
					class="border pl-12 pr-4 w-full py-1.5 text-sm focus:outline-none focus:ring focus:ring-opencord focus:ring-opacity-50 rounded-md"
				/>

				<svg
					xmlns="http://www.w3.org/2000/svg"
					viewBox="0 0 20 20"
					fill="currentColor"
					class="absolute w-4 h-4 -translate-y-1/2 top-1/2 left-4"
				>
					<path
						fill-rule="evenodd"
						d="M8 7a5 5 0 113.61 4.804l-1.903 1.903A1 1 0 019 14H8v1a1 1 0 01-1 1H6v1a1 1 0 01-1 1H3a1 1 0 01-1-1v-2a1 1 0 01.293-.707L8.196 8.39A5.002 5.002 0 018 7zm5-3a.75.75 0 000 1.5A1.5 1.5 0 0114.5 7 .75.75 0 0016 7a3 3 0 00-3-3z"
						clip-rule="evenodd"
					/>
				</svg>
			</div>
		</div>

		<div class="flex justify-center mt-4 space-x-4">
			{#if hasExtension}
				<button
					on:click={logInWithExtension}
					in:fade
					class="text-sm w-full border flex justify-center items-center px-4 py-1.5 hover:bg-opencord transition focus:outline-none focus:ring focus:ring-opencord focus:ring-opacity-50 rounded-md"
				>
					<svg
						xmlns="http://www.w3.org/2000/svg"
						fill="none"
						viewBox="0 0 24 24"
						stroke-width="1.5"
						stroke="currentColor"
						class="w-4 h-4 mr-3"
					>
						<path
							stroke-linecap="round"
							stroke-linejoin="round"
							d="M14.25 6.087c0-.355.186-.676.401-.959.221-.29.349-.634.349-1.003 0-1.036-1.007-1.875-2.25-1.875s-2.25.84-2.25 1.875c0 .369.128.713.349 1.003.215.283.401.604.401.959v0a.64.64 0 01-.657.643 48.39 48.39 0 01-4.163-.3c.186 1.613.293 3.25.315 4.907a.656.656 0 01-.658.663v0c-.355 0-.676-.186-.959-.401a1.647 1.647 0 00-1.003-.349c-1.036 0-1.875 1.007-1.875 2.25s.84 2.25 1.875 2.25c.369 0 .713-.128 1.003-.349.283-.215.604-.401.959-.401v0c.31 0 .555.26.532.57a48.039 48.039 0 01-.642 5.056c1.518.19 3.058.309 4.616.354a.64.64 0 00.657-.643v0c0-.355-.186-.676-.401-.959a1.647 1.647 0 01-.349-1.003c0-1.035 1.008-1.875 2.25-1.875 1.243 0 2.25.84 2.25 1.875 0 .369-.128.713-.349 1.003-.215.283-.4.604-.4.959v0c0 .333.277.599.61.58a48.1 48.1 0 005.427-.63 48.05 48.05 0 00.582-4.717.532.532 0 00-.533-.57v0c-.355 0-.676.186-.959.401-.29.221-.634.349-1.003.349-1.035 0-1.875-1.007-1.875-2.25s.84-2.25 1.875-2.25c.37 0 .713.128 1.003.349.283.215.604.401.96.401v0a.656.656 0 00.658-.663 48.422 48.422 0 00-.37-5.36c-1.886.342-3.81.574-5.766.689a.578.578 0 01-.61-.58v0z"
						/>
					</svg>

					Use Extension
				</button>
			{/if}

			<button
				on:click={generatePrivKey}
				class="text-sm w-full border flex justify-center items-center px-4 py-1.5 hover:bg-opencord transition focus:outline-none focus:ring focus:ring-opencord focus:ring-opacity-50 rounded-md"
			>
				<svg
					xmlns="http://www.w3.org/2000/svg"
					fill="none"
					viewBox="0 0 24 24"
					stroke-width="1.5"
					stroke="currentColor"
					class="w-4 h-4 mr-3"
				>
					<path
						stroke-linecap="round"
						stroke-linejoin="round"
						d="M12 9v6m3-3H9m12 0a9 9 0 11-18 0 9 9 0 0118 0z"
					/>
				</svg>

				Generate Key
			</button>
		</div>

		{#if error}
			<p in:fade class="w-full mt-8 text-sm text-center text-red-500">Error! {error}</p>
		{/if}

		<button
			class="{privkey ??
				'opacity-40 pointer-events-none select-none'} text-sm w-full bg-opencord mt-8 border flex justify-center items-center px-4 py-1.5 hover:bg-emerald-400 transition focus:outline-none focus:ring focus:ring-opencord focus:ring-opacity-50 rounded-md"
		>
			Continue
		</button>
	</div>
</div>
