/*
 * Copyright © Bold Brand Commerce Sp. z o.o. All rights reserved.
 * See LICENSE for license details.
 */
<template>
    <GridTableCell
        :column="0"
        :row="row"
        editing-allowed
        :edit-key-code="32"
        :selected="selected"
        :disabled="disabled"
        @mousedown.native="onSelect"
        @edit="onSelect">
        <GridCheckEditCell
            :value="selected"
            :disabled="disabled" />
    </GridTableCell>
</template>

<script>
import GridCheckEditCell from '@UI/components/Grid/Layout/Table/Cells/Edit/GridCheckEditCell';
import GridTableCell from '@UI/components/Grid/Layout/Table/Cells/GridTableCell';

export default {
    name: 'GridSelectRowEditCell',
    components: {
        GridTableCell,
        GridCheckEditCell,
    },
    props: {
        /**
         * Index of the row
         */
        row: {
            type: Number,
            required: true,
        },
        /**
         * Determines if component is selected
         */
        selected: {
            type: Boolean,
            default: false,
        },
        /**
         * Determines if component is disabled
         */
        disabled: {
            type: Boolean,
            default: false,
        },
    },
    methods: {
        onSelect() {
            if (!this.disabled) {
                this.$emit('select', {
                    row: this.row,
                    selected: !this.selected,
                });
            }
        },
    },
};
</script>
