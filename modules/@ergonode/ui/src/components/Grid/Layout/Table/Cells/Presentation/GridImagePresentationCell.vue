/*
 * Copyright © Bold Brand Commerce Sp. z o.o. All rights reserved.
 * See LICENSE for license details.
 */
<template>
    <div class="grid-image-presentation-cell">
        <LazyImage
            v-if="value"
            :href="`multimedia/${value}/download/default`"
            :value="value" />
    </div>
</template>

<script>
import LazyImage from '@UI/components/LazyImage/LazyImage';

export default {
    name: 'GridImagePresentationCell',
    components: {
        LazyImage,
    },
    props: {
        /**
         * Component value
         */
        value: {
            type: String,
            default: '',
        },
    },
};
</script>

<style lang="scss" scoped>
    .grid-image-presentation-cell {
        display: flex;
        height: 100%;
        padding: 8px;
        box-sizing: border-box;
    }
</style>
