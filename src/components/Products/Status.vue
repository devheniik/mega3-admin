<template>
    <div class="panel">
        <div class="panel-item flex justify-between items-center font-medium">
            Статус
            <Listbox class="float-right" as="div" v-model="selected">
                <ListboxLabel class="sr-only">
                    Изменить опубликованный статус
                </ListboxLabel>
                <div class="relative">
                    <div class="inline-flex shadow-sm rounded-md divide-x divide-indigo-600">
                        <div class="relative z-0 inline-flex shadow-sm rounded-md divide-x divide-indigo-600">
                            <div
                                class="relative inline-flex items-center bg-indigo-500 py-2 pl-3 pr-4 border border-transparent rounded-l-md shadow-sm text-white">
                                <!-- <CheckIcon class="h-5 w-5" aria-hidden="true" /> -->
                                <p class="ml-2.5 text-sm font-medium">{{ selected.title }}</p>
                            </div>
                            <ListboxButton
                                class="relative inline-flex items-center bg-indigo-500 p-2 rounded-l-none rounded-r-md text-sm font-medium text-white hover:bg-indigo-600 focus:outline-none focus:z-10 focus:ring-2 focus:ring-offset-2 focus:ring-offset-gray-50 focus:ring-indigo-500">
                                <ChevronDownIcon class="h-5 w-5 text-white" aria-hidden="true" />
                            </ListboxButton>
                        </div>
                    </div>

                    <transition leave-active-class="transition ease-in duration-100" leave-from-class="opacity-100"
                        leave-to-class="opacity-0">
                        <ListboxOptions
                            class="z-20 origin-top-right absolute right-0 mt-2 w-72 rounded-md shadow-lg overflow-hidden bg-white divide-y divide-gray-200 ring-1 ring-black ring-opacity-5 focus:outline-none">
                            <ListboxOption as="template" v-for="option in publishingOptions" :key="option.title"
                                :value="option" v-slot="{ active, selected }">
                                <li
                                    :class="[active ? 'text-white bg-indigo-500' : 'text-gray-900', 'cursor-default select-none relative p-4 text-sm']">
                                    <div class="flex flex-col">
                                        <div class="flex justify-between">
                                            <p :class="selected ? 'font-semibold' : 'font-normal'">
                                                {{ option.title }}
                                            </p>
                                            <span v-if="selected" :class="active ? 'text-white' : 'text-indigo-500'">
                                                <CheckIcon class="h-5 w-5" aria-hidden="true" />
                                            </span>
                                        </div>
                                        <p :class="[active ? 'text-indigo-200' : 'text-gray-500', 'mt-2']">
                                            {{ option.description }}
                                        </p>
                                    </div>
                                </li>
                            </ListboxOption>
                        </ListboxOptions>
                    </transition>
                </div>
            </Listbox>
        </div>
    </div>
</template>

<script>
    import {
        ref, watchEffect
    } from 'vue'
    const publishingOptions = [{
            title: 'Активный',
            description: 'Этот товар будет доступен для всех каналов продаж.',
            current: true,
            option: 'on_display'
        },
        {
            title: 'Черновик',
            description: 'Этот товар не будет доступен для всех каналов продаж.',
            current: false,
            option: 'draft'
        },
        {
            title: 'Архивный',
            description: 'Этот товар не будет доступен для всех каналов продаж.',
            current: false,
            option: 'archive'
        },
         {
            title: 'Невидимый',
            description: 'Этот товар не будет видим для всех каналов продаж.',
            current: false,
            option: 'not_on_display'
        },
    ]

    export default {
        setup(props, { emit }) {
            const selected = ref(publishingOptions[0])

            watchEffect(() => emit('change', selected.value.option))



            return {
                publishingOptions,
                selected
            }
        }
    }
</script>

<style lang="scss" scoped>
</style>