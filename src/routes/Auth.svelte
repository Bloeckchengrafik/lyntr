<script lang="ts">
	import * as AlertDialog from '@/components/ui/alert-dialog';
	import { Button } from '@/components/ui/button';
	import Label from '@/components/ui/label/label.svelte';
	import { supabase } from '@/supabase';
	import { mode } from 'mode-watcher';

	let loading = false;

	const handleLogin = async () => {
		try {
			loading = true;
			const { error } = await supabase.auth.signInWithOAuth({
				provider: 'discord'
			});

			if (error) throw error;
		} catch (error) {
			if (error instanceof Error) {
				alert(error.message);
			}
		} finally {
			loading = false;
		}
	};
</script>

<div class="flex h-screen">
	<div class="m-auto flex flex-col items-center gap-2">
		<div class="inline-flex items-center gap-2">
			<img src="logo.svg" alt="Lyntr" class="pointer-events-none h-40 w-40 select-none" />
			<Label class="select-none text-8xl">Lyntr.</Label>
		</div>

		<Label class="text-3xl">Please log in to begin.</Label>

		<div class="flex w-full items-center justify-center space-x-2">
			<AlertDialog.Root>
				<AlertDialog.Trigger asChild let:builder>
					<Button builders={[builder]} on:click={handleLogin} disabled={loading}>
						<img
							src={$mode === 'dark'
								? 'https://cdn.prod.website-files.com/6257adef93867e50d84d30e2/653714c17467993e7b389c83_636e0a6918e57475a843f59f_icon_clyde_black_RGB.svg'
								: 'https://cdn.prod.website-files.com/6257adef93867e50d84d30e2/653714c1f22aef3b6921d63d_636e0a6ca814282eca7172c6_icon_clyde_white_RGB.svg'}
							alt="Discord"
							class="h-5 w-5 mr-2"
							/>
						{loading ? 'Loading' : 'Login with Discord'}</Button
					>
				</AlertDialog.Trigger>
				<AlertDialog.Content>
					<AlertDialog.Header>
						<AlertDialog.Title>You are soon to be redirected.</AlertDialog.Title>
						<AlertDialog.Description>
							Once you authorise with Discord, you will be redirected back to Lyntr.com
						</AlertDialog.Description>
					</AlertDialog.Header>
				</AlertDialog.Content>
			</AlertDialog.Root>
		</div>
	</div>
</div>
