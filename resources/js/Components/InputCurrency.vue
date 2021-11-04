<template>
<div>
    <jet-label v-if="label" :for="id" :value="label" />
    <money3
        v-model.number="amount"
        v-bind="config"
        :id="id"
        :class="classes + extraclass"
    />
    <jet-input-error v-if="error" :message="error" class="mt-2" />
</div>
</template>

<script>
    import { defineComponent } from 'vue'
    import JetLabel from '@/Jetstream/Label.vue'
    import JetInputError from '@/Jetstream/InputError.vue'
    import { Money3Component } from "v-money3";

    export default defineComponent({
        inheritAttrs: false,
        components: {
            JetLabel,
            JetInputError,
            money3: Money3Component,
        },
        props: {
            modelValue: String,
            error: [Boolean, String],
            id: String,
            type: String,
            label: String,
            extraclass: String,
            percent: {
                type: Boolean,
                default: false
            },
        },
        emits: ['update:modelValue'],
        data() {
            return {
                amount: 0,
                config: {
                    masked: false,
                    prefix: this.percent ? '' : 'R$ ',
                    suffix: this.percent ? '%' : '',
                    thousands: '.',
                    decimal: ',',
                    precision: 2,
                    disableNegative: false,
                    disabled: false,
                    min: null,
                    max: this.percent ? 100 : null,
                    allowBlank: false,
                    minimumNumberOfCharacters: 0,
                    modelModifiers: {
                        number: true,
                    },
                },
            }
        },
        watch: {
            modelValue(val) {
                this.amount = val;
            },
            amount(val) {
                this.$emit('update:modelValue', val);
            },
        },
        computed: {
            classes() {
                return this.error ?
                'focus:ring-opacity-50 rounded-md shadow-sm border-red-600 focus:border-red-800 focus:ring focus:ring-red-200 dark:border-gray-600 dark:bg-gray-700 dark:text-gray-300 dark:focus:ring-gray ':
                'focus:ring-opacity-50 rounded-md shadow-sm border-gray-300 focus:border-indigo-300 focus:ring focus:ring-indigo-200 dark:border-gray-600 dark:bg-gray-700 dark:text-gray-300 dark:focus:ring-gray ';
            }
        }
    })
</script>
