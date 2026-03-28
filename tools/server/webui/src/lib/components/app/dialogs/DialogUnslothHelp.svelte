<script lang="ts">
	import * as AlertDialog from '$lib/components/ui/alert-dialog';
	import { ExternalLink, Info } from '@lucide/svelte';

	let { open = $bindable(false) } = $props();
</script>

<AlertDialog.Root bind:open>
	<AlertDialog.Portal>
		<AlertDialog.Overlay />

		<AlertDialog.Content class="flex max-w-2xl flex-col">
			<AlertDialog.Header>
				<AlertDialog.Title class="flex items-center gap-2">
					<Info class="h-5 w-5 text-primary" />
					Fixing Unsloth to GGUF Conversion Issues
				</AlertDialog.Title>

				<AlertDialog.Description class="text-sm text-muted-foreground">
					Common solutions for problems when exporting Unsloth fine-tuned models to GGUF.
				</AlertDialog.Description>
			</AlertDialog.Header>

			<div class="!max-h-[60vh] min-h-0 flex-1 space-y-6 overflow-y-auto py-4">
				<section class="space-y-3">
					<h3 class="text-base font-semibold">1. Merge to 16-bit first</h3>
					<p class="text-sm text-muted-foreground">
						Direct GGUF export can sometimes fail due to memory issues or library version
						mismatches. The most reliable method is to merge your LoRA adapters to 16-bit
						precision first:
					</p>
					<pre class="overflow-x-auto rounded-lg bg-muted p-3 font-mono text-xs">
model.save_pretrained_merged("model_16bit", tokenizer, save_method = "merged_16bit")</pre
					>
				</section>

				<section class="space-y-3">
					<h3 class="text-base font-semibold">2. Use "GGUF My Repo" Space</h3>
					<p class="text-sm text-muted-foreground">
						After saving as 16-bit and uploading to Hugging Face, use the official GGUF
						conversion space. This avoids local setup issues with llama.cpp:
					</p>
					<a
						href="https://huggingface.co/spaces/ggml-org/gguf-my-repo"
						target="_blank"
						rel="noopener noreferrer"
						class="flex items-center gap-2 text-sm font-medium text-primary hover:underline"
					>
						Hugging Face GGUF-my-repo <ExternalLink class="h-3 w-3" />
					</a>
				</section>

				<section class="space-y-3">
					<h3 class="text-base font-semibold">3. Update llama.cpp scripts</h3>
					<p class="text-sm text-muted-foreground">
						If converting locally, ensure you have the latest <code>convert_hf_to_gguf.py</code> from
						the llama.cpp repository. Unsloth often relies on recent changes in the conversion scripts.
					</p>
					<pre class="overflow-x-auto rounded-lg bg-muted p-3 font-mono text-xs">
python3 convert_hf_to_gguf.py ./model_16bit --outfile model.gguf</pre
					>
				</section>

				<section class="space-y-3">
					<h3 class="text-base font-semibold">4. Check for NaN values</h3>
					<p class="text-sm text-muted-foreground">
						If your converted model produces gibberish, check if your fine-tuning had loss spikes
						resulting in NaNs. Quantization (especially 4-bit) is very sensitive to weight
						distribution.
					</p>
				</section>
			</div>

			<AlertDialog.Footer>
				<AlertDialog.Action onclick={() => (open = false)}>Close</AlertDialog.Action>
			</AlertDialog.Footer>
		</AlertDialog.Content>
	</AlertDialog.Portal>
</AlertDialog.Root>
