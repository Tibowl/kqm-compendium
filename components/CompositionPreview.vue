<template>
    <div class="relative mb-3">
        <NuxtLink :to="`/${composition.dir.split('/').pop()}`">
            <div :class="'bg-[#28242c] h-full p-5 pb-8 rounded-xl text-white ' + (!detailedView ? 'flex flex-row' : '')">
                <div :class="'relative flex pb-2 ' + (detailedView ? 'w-full justify-between' : 'w-1/3 flex-col text-center justify-center items-center pr-3')">
                    <div class="text-xl lg:text-3xl font-bold font-genshin">
                        {{ composition.title }}
                    </div>
                    <div class="flex justify-end items-end">
                        <p class="text-gray-500 lg:text-xl">
                            {{ composition.authors.join(', ') }}
                        </p>
                    </div>
                </div>

                <!-- Desktop -->
                <div :class="'hidden ' + (detailedView ? 'lg:block' : 'lg:flex flex-row items-center')">
                    <div class="w-full grid grid-cols-4 gap-2 justify-center py-3">
                        <div
                            v-for="character of composition.characters"
                            :key="character.slug"
                        >
                            <div
                                class="bg-cover rounded-t-lg relative"
                                :style="{ backgroundImage: `url(${require(`~/assets/icons/bg-tier${character.star}.png`)})` }"
                            >
                                <img :src="require(`~/assets/character_thumbnails/${character.name}.png`)" class="mx-auto">
                                <div class="absolute bottom-1 w-full text-center flex flex-row justify-center">
                                    <div class="w-6 h-6 flex justify-center items-center bg-purple-700 rotate-45 rounded-sm z-10">
                                        <p class="w-full h-full items-center justify-center flex font-bold -rotate-45">
                                            {{ `C${character.constellation}` }}
                                        </p>
                                    </div>
                                    <div class="absolute -bottom-3 w-full h-4 bg-gray-700 rounded-[50%]" />
                                </div>
                            </div>
                            <div class="w-full bg-gray-700 relative z-10 rounded-b-lg grid grid-cols-2 justify-center items-center gap-0.5 p-1">
                                <div class="bg-gray-900 w-full h-full rounded-lg flex flex-row justify-center items-center">
                                    <img
                                        :src="require(`~/assets/weapons/${character.weapon.name}.png`)"
                                        :title="character.weapon.name"
                                        class="inline w-1/2"
                                    >
                                    <p class="text-xs font-bold">
                                        {{ `R${character.weapon.refinement}` }}
                                    </p>
                                </div>
                                <div class="bg-gray-900 w-full h-full rounded-lg flex flex-row justify-center items-center">
                                    <img
                                        v-for="artifact of character.artifacts"
                                        :key="artifact"
                                        :src="require(`~/assets/artifacts/${artifact}/flower.png`)"
                                        :title="artifact"
                                        class="inline w-1/2"
                                    >
                                </div>
                            </div>
                            <div class="flex justify-center text-center font-genshin pt-1">
                                {{ character.name }}
                            </div>
                        </div>
                    </div>
                    <div class="w-full flex flex-row justify-evenly p-3 pt-0 text-center">
                        <div
                            class="flex flex-col items-center font-bold"
                            :class="{1: 'text-red-400', 2: 'text-yellow-400', 3: 'text-green-400'}[composition.healing]"
                        >
                            <img src="~/assets/icons/healing.png" class="h-10">
                            {{ {1: 'No Healing', 2: 'Some Healing', 3: 'Healing'}[composition.healing] }}
                        </div>
                        <div
                            class="flex flex-col items-center font-bold"
                            :class="{1: 'text-red-400', 2: 'text-yellow-400', 3: 'text-green-400'}[composition.shielding]"
                        >
                            <img src="~/assets/icons/shield.png" class="h-10">
                            {{ {1: 'No Shielding', 2: 'Some Shielding', 3: 'Shielding'}[composition.shielding] }}
                        </div>
                        <div
                            class="flex flex-col items-center font-bold"
                            :class="{1: 'text-green-400', 2: 'text-yellow-400', 3: 'text-red-400', 4: 'text-red-600'}[composition.difficulty]"
                        >
                            <img :src="require(`~/assets/icons/star_${composition.difficulty}.png`)" class="h-10">
                            {{ {1: 'Easy', 2: 'Medium', 3: 'Difficult', 4: 'Very Difficult'}[composition.difficulty] }}
                        </div>
                    </div>
                    <div v-if="detailedView" class="w-full py-3 text-gray-500">
                        {{ composition.tags.join(', ') }}
                    </div>
                    <div v-if="detailedView" class="grid grid-cols-2 gap-5">
                        <div class="text-xl font-bold">
                            Pros
                            <ul class="pl-5 text-gray-500">
                                <li v-for="pro of composition.pros" :key="pro" class="text-base font-normal list-disc">
                                    {{ pro }}
                                </li>
                            </ul>
                        </div>
                        <div class="text-xl font-bold">
                            Cons
                            <ul class="pl-5 text-gray-500">
                                <li v-for="con of composition.cons" :key="con" class="text-base font-normal list-disc">
                                    {{ con }}
                                </li>
                            </ul>
                        </div>
                    </div>
                </div>

                <!-- Mobile -->
                <div class="lg:hidden">
                    <div class="flex flex-row justify-center items-center">
                        <div class="flex flex-row flex-wrap gap-2 justify-center">
                            <div
                                v-for="character of composition.characters"
                                :key="character.slug"
                                class="w-5/12"
                            >
                                <div
                                    class="bg-cover rounded-t-lg relative"
                                    :style="{ backgroundImage: `url(${require(`~/assets/icons/bg-tier${character.star}.png`)})` }"
                                >
                                    <img :src="require(`~/assets/character_thumbnails/${character.name}.png`)" class="mx-auto">
                                    <div class="absolute bottom-1 w-full text-center flex flex-row justify-center">
                                        <div class="w-6 h-6 flex justify-center items-center bg-purple-700 rotate-45 rounded-sm z-10">
                                            <p class="w-full h-full items-center justify-center flex font-bold -rotate-45">
                                                {{ `C${character.constellation}` }}
                                            </p>
                                        </div>
                                        <div class="absolute -bottom-3 w-full h-4 bg-gray-700 rounded-[50%]" />
                                    </div>
                                </div>
                                <div class="w-full bg-gray-700 relative z-10 rounded-b-lg flex justify-center items-center gap-0.5 p-1">
                                    <img
                                        :src="require(`~/assets/weapons/${character.weapon.name}.png`)"
                                        :title="character.weapon.name"
                                        class="inline w-1/3"
                                    >
                                    <img
                                        v-for="artifact of character.artifacts"
                                        :key="artifact"
                                        :src="require(`~/assets/artifacts/${artifact}/flower.png`)"
                                        :title="artifact"
                                        class="inline w-1/3"
                                    >
                                </div>
                                <div class="flex justify-center text-center text-sm font-genshin pt-1">
                                    {{ character.name }}
                                </div>
                            </div>
                        </div>
                        <div class="max-w-[25%] w-full flex flex-col justify-evenly text-center gap-3">
                            <div
                                class="flex flex-col items-center font-bold"
                                :class="{1: 'text-red-400', 2: 'text-yellow-400', 3: 'text-green-400'}[composition.healing]"
                            >
                                <img src="~/assets/icons/healing.png" class="max-h-10">
                                {{ {1: 'No Healing', 2: 'Some Healing', 3: 'Healing'}[composition.healing] }}
                            </div>
                            <div
                                class="flex flex-col items-center font-bold"
                                :class="{1: 'text-red-400', 2: 'text-yellow-400', 3: 'text-green-400'}[composition.shielding]"
                            >
                                <img src="~/assets/icons/shield.png" class="max-h-10">
                                {{ {1: 'No Shielding', 2: 'Some Shielding', 3: 'Shielding'}[composition.shielding] }}
                            </div>
                            <div
                                class="flex flex-col items-center font-bold"
                                :class="{1: 'text-green-400', 2: 'text-yellow-400', 3: 'text-red-400'}[composition.difficulty]"
                            >
                                <img :src="require(`~/assets/icons/star_${composition.difficulty}.png`)" class="max-h-10">
                                {{ {1: 'Easy', 2: 'Medium', 3: 'Difficult'}[composition.difficulty] }}
                            </div>
                        </div>
                    </div>
                </div>
            </div>

            <div class="lg:hidden absolute left-0 -bottom-4 w-full flex justify-center items-center">
                <div class="w-3/12 h-8 bg-purple-700 flex justify-center items-center text-xl text-white font-bold rounded-md">
                    Details
                </div>
            </div>
        </NuxtLink>

        <div class="hidden absolute left-0 -bottom-4 w-full lg:flex justify-center items-center gap-5">
            <a
                v-if="composition.sheet_url"
                class="w-3/12 h-8 bg-green-500 flex justify-center items-center text-xl text-white font-bold rounded-md"
                :href="composition.sheet_url"
                target="_blank"
            >
                Sheet
            </a>
            <a
                v-if="composition.gcsim_url"
                class="w-3/12 h-8 bg-blue-600 flex justify-center items-center text-xl text-white font-bold rounded-md"
                :href="composition.gcsim_url"
                target="_blank"
            >
                gcsim
            </a>
        </div>
    </div>
</template>

<script lang="ts">
import Vue from 'vue'

export default Vue.extend({
    name: 'CompositionPreview',
    props: {
        composition: {
            type: Object,
            required: true
        },
        detailedView: {
            type: Boolean,
            required: true
        }
    }
})
</script>
