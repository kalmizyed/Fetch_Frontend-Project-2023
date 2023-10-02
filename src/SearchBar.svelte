<script>
    import { onMount } from 'svelte';
    import axios from 'axios';
    import Tags from "svelte-tags-input";

    export let updateQuery;
    export let text;

    let tags = [];
    $: updateQuery(tags);

    let breedList = [];
    let dogListURL = 'https://dog.ceo/api/breeds/list/all';
    onMount(async () => {
        const response = await axios.get(dogListURL);
        breedList = Object.keys(response.data.message);
    });
</script>

<Tags
    bind:tags={tags}
    autoComplete={breedList}
    placeholder={text}
    onlyAutocomplete={true}
    onlyUnique={true}
/>