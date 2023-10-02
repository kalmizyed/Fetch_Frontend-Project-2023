<script>
    import axios from 'axios';
    import SearchBar from './SearchBar.svelte';
    import { Gallery } from 'flowbite-svelte';

    let queriedBreeds = [];
    function updateQuery(query) {
        queriedBreeds = query;
        updateImages();
    }

    // Gallery update functions

    let images = [];
    /**
     * Fetches images of a given dog breed from the Dog API.
     * @param breed {string} The name of the breed to be fetched
     */
    async function fetchImages(breed) {
        let breedImageURL = `https://dog.ceo/api/breed/${breed}/images`;
        const response = await axios.get(breedImageURL);
        return response.data.message.map((image) => {
            return {
                alt: `Image of a ${breed}`,
                src: image
            }
        })
    }

    /**
     * Updates the gallery to match the current search query.
     */
    async function updateImages() {
        images = [];
        queriedBreeds.forEach(async breed => {
            let breedImages = await fetchImages(breed);
            images = [...images, ...breedImages];
        });
    }
</script>

<main>
    <SearchBar 
        updateQuery={updateQuery}
        text={'Enter a dog breed...'}
    />
    <Gallery
        items={images}
        class="gap-4 grid-cols-2 md:grid-cols-6"
    />
</main>