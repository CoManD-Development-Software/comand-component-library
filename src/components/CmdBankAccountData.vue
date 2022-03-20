<template>
    <div class="cmd-bank-account-data">
        <!-- begin CmdCustomHeadline -->
        <CmdCustomHeadline v-if="cmdCustomHeadline" v-bind="cmdCustomHeadline"/>
        <!-- end CmdCustomHeadline -->

        <!-- begin account data -->
        <dl>
            <template v-for="(entry, index) in accountData" :key="index">
                <dt>{{ entry.text }}</dt>
                <dd v-if="allowCopyByClick && entry.allowCopy" class="flex-container no-flex">
                    <span>{{ entry.value }}</span>
                    <a href="#" @click.prevent="copyToClipboard(entry.value)" :title="iconCopy.tooltip">
                        <span :class="iconCopy.iconClass"></span>
                    </a>
                </dd>
                <dd v-else :key="index">{{ entry.value }}</dd>
            </template>
        </dl>
        <!-- end account data -->

        <!-- begin additional information -->
        <p v-if="additionalInformation">{{ additionalInformation }}</p>
        <!-- end additional information -->
    </div>
</template>

<script>
// import components
import CmdCustomHeadline from "./CmdCustomHeadline"

export default {
    name: "CmdBankAccountData",
    components: {CmdCustomHeadline},
    props: {
        /**
         * bank account data
         *
         * must contain: owner, name of bank, IBAN, SWIFT/BIC
         */
        accountData: {
            type: Array,
            required: true
        },
        /**
         * enable if data can be copied by click on icon
         */
        allowCopyByClick: {
            type: Boolean,
            default: true
        },
        /**
         * additional information shown in a paragraph below account data
         */
        additionalInformation: {
            type: String,
            required: false
        },
        /**
         * icon 'copy'
         *
         * @requiredForAccessibility: partial
         */
        iconCopy: {
            type: Object,
            default() {
                return {
                    iconClass: "icon-file-copy",
                    tooltip: "Copy data to clipboard!"
                }
            }
        },
        /**
         * properties for CmdCustomHeadline-component
         */
        cmdCustomHeadline: {
            type: Object,
            required: false
        }
    },
    methods: {
        copyToClipboard(entry) {
            // get data, remove spaces and copy to clipboard
            const output = entry.replace(/\s/g, "")
            navigator.clipboard.writeText(output)
            alert(output + " was copied to clipboard")
        }
    }
}
</script>

<style lang="scss">
.cmd-bank-account-data {
    dd.flex-container {
        gap: calc(var(--default-gap) / 2);

        > span {
            &:first-child {
                white-space: nowrap;
            }
        }
    }
}
</style>