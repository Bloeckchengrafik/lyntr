<script lang="ts">
    import { Label } from '@/components/ui/label';
    import { fly } from 'svelte/transition';
    import { quintOut } from 'svelte/easing';

    export let tabs: string[];
    export let currentTab: string;
    export let onTabChange: (tab: string) => void;
</script>

<div class="flex justify-center gap-20">
    {#each tabs as tab}
        <div class="relative">
            <Label 
                class="cursor-pointer select-none text-xl" 
                on:mousedown={() => onTabChange(tab)}
            >
                {tab}
            </Label>
            {#if currentTab === tab}
                <div 
                    class="absolute -bottom-2 left-0 right-0 mt-4 h-2 rounded-b-lg bg-primary"
                    in:fly="{{ y: 10, duration: 200, easing: quintOut }}"
                    out:fly="{{ y: 10, duration: 200, easing: quintOut }}"
                ></div>
            {/if}
        </div>
    {/each}
</div>