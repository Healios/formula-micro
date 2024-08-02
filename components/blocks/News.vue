<script setup lang="ts">
    import { ref, computed, onMounted } from 'vue';
    import type { News_Blocks } from "@/graphql/generated/graphql";
    import { useNewsPostsByTagsQuery } from "@/graphql/generated/graphql";

    // Properties.
    interface Properties {
        data: News_Blocks;
    }
    const props = defineProps<Properties>();

    // Reactive fields.
    const newsPosts = ref<any>(null);
    const { __typename, title, tags } = toRefs(props.data);
    const tagIds = computed(() => props.data.tags?.map(tag => tag.tags_id.id) ?? []);
    console.log(tagIds.value);
    

    const { data, error } = await useNewsPostsByTagsQuery({ variables: { tags: tagIds.value } });
    console.log(data.value);

    /*
    // query i postman, som virker:
        query NewsPostsByTags($tags: [String]) {
            news_posts(filter: { tags: { tags_id: { id: { _in: $tags } } } }) {
                id
                title
            }
        }

    // GRAPHQL VARIABLES:
    {
    "tags": ["dc18a13d-4126-4f4e-aca9-812fa010511c"]
    }
    */

</script>

<template>
    <div v-if="__typename === 'news_blocks'" class="max-w-7xl px-6 xl:px-12">
        <h1>Test</h1>
    </div>
</template>

