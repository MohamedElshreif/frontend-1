/*
 * Copyright © Bold Brand Commerce Sp. z o.o. All rights reserved.
 * See LICENSE for license details.
 */
<template>
    <NotificationListItemTemplate :item="item">
        <template #prepend>
            <CircleProgressBar
                :radius="radius"
                :progress="progress"
                :stroke="stroke" />
        </template>
    </NotificationListItemTemplate>
</template>

<script>
import NotificationListItemTemplate from '@Notifications/components/NotificationList/Item/NotificationListItemTemplate';
import CircleProgressBar from '@UI/components/ProgressBar/Circle/CircleProgressBar';

export default {
    name: 'NotificationListImportProcessingItem',
    components: {
        NotificationListItemTemplate,
        CircleProgressBar,
    },
    props: {
        item: {
            type: Object,
            required: true,
        },
    },
    computed: {
        radius() {
            return 20;
        },
        progress() {
            return (this.item.processed / this.item.items) * 100;
        },
        stroke() {
            return 2;
        },
    },
};
</script>
