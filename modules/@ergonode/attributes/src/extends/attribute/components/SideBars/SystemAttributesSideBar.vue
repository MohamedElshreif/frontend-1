/*
 * Copyright © Bold Brand Commerce Sp. z o.o. All rights reserved.
 * See LICENSE for license details.
 */
<template>
    <SideBar
        :title="$t('@Attributes.attribute.components.SystemAttributesSideBar.title')"
        :items="attributesByLanguage"
        :searchable="true"
        :search-value="searchValue"
        @search="onSearch">
        <template #header>
            <ListSearchSelectHeader
                v-if="isSelectLanguage"
                :title="$t('@Attributes.attribute.components.SystemAttributesSideBar.title')"
                :search-value="searchValue"
                @search="onSearch">
                <template #select>
                    <LanguageTreeSelect
                        :value="languageCode"
                        @input="onSelectLanguage" />
                </template>
            </ListSearchSelectHeader>
        </template>
        <template #body>
            <Preloader v-if="isPrefetchingData" />
        </template>
        <template #item="{ item }">
            <AttributeSideBarElement
                :item="item"
                :language-code="languageCode"
                :disabled="disabled" />
        </template>
    </SideBar>
</template>

<script>
import AttributeSideBarElement from '@Attributes/extends/attribute/components/SideBars/AttributeSideBarElement';
import LanguageTreeSelect from '@Core/components/Selects/LanguageTreeSelect';
import {
    deepClone,
} from '@Core/models/objectWrapper';
import {
    getItems,
} from '@Core/services/sidebar';
import ListSearchSelectHeader from '@UI/components/List/ListSearchSelectHeader';
import Preloader from '@UI/components/Preloader/Preloader';
import SideBar from '@UI/components/SideBar/SideBar';
import debounce from 'debounce';
import {
    mapState,
} from 'vuex';

export default {
    name: 'SystemAttributesSideBar',
    components: {
        Preloader,
        ListSearchSelectHeader,
        AttributeSideBarElement,
        SideBar,
        LanguageTreeSelect,
    },
    props: {
        isSelectLanguage: {
            type: Boolean,
            default: true,
        },
        disabled: {
            type: Boolean,
            default: false,
        },
    },
    async fetch() {
        await this.getItems();

        this.isPrefetchingData = false;
    },
    data() {
        return {
            isPrefetchingData: true,
            attributes: {},
            attributesBeforeSearch: {},
            languageCode: '',
            searchValue: '',
            onDebounceGetItems: null,
        };
    },
    computed: {
        ...mapState('authentication', {
            languagePrivileges: state => state.user.languagePrivileges,
        }),
        ...mapState('core', [
            'defaultLanguageCode',
        ]),
        attributesByLanguage() {
            return this.attributes[this.languageCode] || [];
        },
    },
    created() {
        this.languageCode = this.defaultLanguageCode;

        this.onDebounceGetItems = debounce(this.getItems, 500);
    },
    methods: {
        async getItems() {
            const params = {
                limit: 99999,
                offset: 0,
                view: 'list',
                field: 'code',
                order: 'ASC',
            };

            if (this.searchValue !== '') {
                params.filter = `code=${this.searchValue}`;
            }

            await getItems({
                $axios: this.$axios,
                languageCode: this.languageCode,
                path: `${this.languageCode}/attributes/system`,
                params,
                onSuccess: this.getItemsSuccess,
            });
        },
        getItemsSuccess({
            items,
            languageCode,
        }) {
            this.attributes = {
                ...this.attributes,
                [languageCode]: items,
            };
        },
        async onSearch(value) {
            if (this.searchValue === '') {
                this.attributesBeforeSearch = deepClone(this.attributes);
            }

            this.searchValue = value;

            if (value !== '') {
                this.onDebounceGetItems();
            } else {
                this.attributes = deepClone(this.attributesBeforeSearch);
                this.attributesBeforeSearch = {};
            }
        },
        async onSelectLanguage(value) {
            this.languageCode = value;

            if (typeof this.attributes[value] === 'undefined') {
                this.isPrefetchingData = true;

                await this.getItems();

                this.isPrefetchingData = false;
            }
        },
    },
};
</script>
