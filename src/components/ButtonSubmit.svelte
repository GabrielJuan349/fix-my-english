<script>
    import { isValidInput } from "./store.js"

    import Loading from "./icons/Loading.svelte"
    import Upload from "./icons/Upload.svelte"
    import {fixMyEnglish} from "../services/ia.js"
    let promise = null

    const handleClick= async () => {
        const text = document.getElementById("result").value
        promise = fixMyEnglish(text)
        const value = await promise

        document.getElementById("result").value = value
        promise = null
    }
</script>

{#if promise === null}
    <button 
    on:click={handleClick}
    disabled={!$isValidInput}
    type="button" class={`py-2 px-4 flex justify-center items-center  bg-red-600 hover:bg-red-700 focus:ring-red-500 focus:ring-offset-red-200 text-white 
    w-full transition ease-in duration-200 text-center text-base font-semibold shadow-md focus:outline-none focus:ring-2 focus:ring-offset-2  rounded-lg ${
        !$isValidInput ? "pointer-events-none opacity-30" : ""
    }`} >
    <Upload />
        Fix my English
    </button>

{:else}
    {#await promise}
        <button 
        type="button" 
        class="pointer-events-none py-2 px-4 flex justify-center items-center  bg-blue-600 hover:bg-blue-700 focus:ring-blue-500 focus:ring-offset-blue-200
        text-white w-full transition ease-in duration-200 text-center text-base font-semibold shadow-md focus:outline-none focus:ring-2 focus:ring-offset-2  rounded-lg ">
            <Loading />
            Loading ...
        </button>

    {/await}
{/if}

{#if !$isValidInput}
    <strong class="text-red-800"> 
        You are not using English or the text is too short!
    </strong>
{/if}