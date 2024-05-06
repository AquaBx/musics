<script lang="ts">
    import { index } from "./store";

    let {lib} = $props()

</script>

<table class="border-collapse table-auto w-full text-sm">
    <tbody class="bg-white dark:bg-slate-800">
        {#each lib as l,i}
            <tr class:bg-green={i==$index}  onclick={() => $index=i} class="cursor-pointer">
                <td class="border-b border-slate-200 dark:border-slate-600 p-4 pl-8 text-slate-500 dark:text-slate-400">    
                    <i class="ph ph-play"></i>
                </td>
                <td class="border-b border-slate-200 dark:border-slate-600 p-4 pl-8 text-slate-500 dark:text-slate-400">    
                    {l}
                </td>
            </tr>
        {/each}
    </tbody>
</table>

<style>
    .bg-green{
        background:#00ff5522;
    }
</style>