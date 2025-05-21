<template>
    <div class="mx-auto px-16 py-4 w-full max-w-7xl">
        <div class="flex justify-between items-center mb-8">
            <h1 class="font-bold text-3xl">
                Malaysia License Plate Generator
            </h1>
            <button 
                @click="toggleTheme" 
                class="flex items-center gap-2 bg-gray-100 hover:bg-gray-200 dark:bg-gray-800 dark:hover:bg-gray-700 px-4 py-2 rounded-lg text-gray-700 dark:text-gray-300 transition-colors"
            >
                <Icon :name="isDark ? 'ph:moon-fill' : 'ph:sun-fill'" class="text-xl" />
                {{ isDark ? 'Dark' : 'Light' }} Mode
            </button>
        </div>

        <!-- Plate Preview -->
        <div class="bg-white dark:bg-gray-800 shadow-sm mb-12 border-4 border-gray-100 dark:border-gray-700 rounded-xl rounded-t-lg">
            <h2 class="bg-gray-100 dark:bg-gray-700 mb-4 p-2 overflow-hidden font-semibold dark:text-white text-xl">Preview</h2>
            <div class="relative mx-auto w-full max-w-[800px] h-[176px]">
                <!-- Main plate content -->
                <div class="absolute inset-0 rounded-lg" :style="{
                    backgroundColor: backgroundColors[backgroundColor]
                }">
                    <!-- QR Code (top right) -->
                    <div class="top-2 right-2 absolute bg-black rounded-sm w-4 h-4"></div>

                    <!-- Serial Number (top right) -->
                    <div class="top-2 right-8 absolute text-[8px] text-gray-400">000007543</div>

                    <!-- "FRONT" text (bottom right) -->
                    <div class="right-2 bottom-2 absolute text-[8px] text-gray-400">FRONT</div>

                    <div class="flex flex-row h-full">
                        <!-- Decal Area with Malaysian Flag -->
                        <div class="relative pl-2.5 rounded-s-xl overflow-hidden" :style="{
                            backgroundColor: plateType === 'ev' ? '#8dbf22' : '#0000B8'
                        }">
                            <div class="flex justify-center items-center mx-auto p-3 w-full h-[80px]">
                                <img src="https://i.imgur.com/MPgmBgw.png" alt="flag"
                                    class="w-full h-full object-contain" />
                            </div>
                            <div
                                class="flex justify-center items-center h-[81px] font-montserrat font-bold text-white text-xl tracking-wider">
                                MAL
                            </div>
                        </div>

                        <!-- Secure Strip -->
                        <div class="relative flex justify-center items-center bg-[#dbdbdb] w-[10px] overflow-hidden">
                            <span class="text-gray-400 text-xs text-center rotate-180 [writing-mode:vertical-rl]">
                                SECURE SECURE SECURE
                            </span>
                        </div>

                        <!-- Plate Text Area -->
                        <div class="flex flex-1 justify-center items-center">
                            <div class="text-8xl euro-font" :style="{
                                color: textColors[textColor],
                                letterSpacing: '4px'
                            }">
                                {{ plateText }}
                            </div>
                        </div>
                    </div>
                </div>

                <!-- Border effect overlay -->
                <div class="absolute inset-0 rounded-lg pointer-events-none" :style="{
                    boxShadow: `
                            inset 0 0 0 8px #e2e2e2,
                            inset 2px 2px 4px rgba(0, 0, 0, 0.1),
                            inset -1px -1px 3px rgba(255, 255, 255, 0.3)
                        `
                }"></div>
            </div>
        </div>

        <!-- Controls -->
        <div class="bg-white dark:bg-gray-800 shadow-sm mb-12 border-4 border-gray-100 dark:border-gray-700 rounded-xl rounded-t-lg">
            <h2 class="bg-gray-100 dark:bg-gray-700 mb-4 p-2 overflow-hidden font-semibold dark:text-white text-xl">Controls</h2>
            <div class="gap-6 grid grid-cols-1 md:grid-cols-2 p-4">
                <div>
                    <label class="block mb-2 text-lg">Plate Type</label>
                    <div class="flex flex-wrap gap-2">
                        <button @click="plateType = 'normal'" :class="[
                            'px-4 py-2 rounded-lg text-sm',
                            plateType === 'normal' ? 'bg-blue-600 text-white' : 'bg-gray-100'
                        ]">
                            Normal
                        </button>
                        <button @click="plateType = 'ev'" :class="[
                            'px-4 py-2 rounded-lg text-sm',
                            plateType === 'ev' ? 'bg-green-600 text-white' : 'bg-gray-100'
                        ]">
                            EV
                        </button>
                        <button @click="plateType = 'commercial'" :class="[
                            'px-4 py-2 rounded-lg text-sm',
                            plateType === 'commercial' ? 'bg-yellow-600 text-white' : 'bg-gray-100'
                        ]">
                            Commercial
                        </button>
                        <button @click="plateType = 'military'" :class="[
                            'px-4 py-2 rounded-lg text-sm',
                            plateType === 'military' ? 'bg-green-800 text-white' : 'bg-gray-100'
                        ]">
                            Military
                        </button>
                        <button @click="plateType = 'public'" :class="[
                            'px-4 py-2 rounded-lg text-sm',
                            plateType === 'public' ? 'bg-red-600 text-white' : 'bg-gray-100'
                        ]">
                            Public Transport
                        </button>
                    </div>
                </div>

                <div>
                    <label class="block mb-2 text-lg">Plate Number</label>
                    <input v-model="plateText" type="text" maxlength="10"
                        class="p-4 border rounded-xl w-full text-xl uppercase" />
                </div>

                <div>
                    <label class="block mb-2 text-lg">Font Style</label>
                    <select v-model="fontStyle" class="p-4 border rounded-xl w-full">
                        <option value="eu">European Font</option>
                        <option value="uk">UK/Singapore Font</option>
                        <option value="old">Old Malaysian Font</option>
                        <option value="japan">Japanese Font</option>
                    </select>
                </div>

                <div>
                    <label class="block mb-2 text-lg">Text Color</label>
                    <select v-model="textColor" class="p-4 border rounded-xl w-full">
                        <option v-for="(_, color) in textColors" :key="color" :value="color">
                            {{ color.charAt(0).toUpperCase() + color.slice(1) }}
                        </option>
                    </select>
                </div>
            </div>
        </div>

        <!-- Disclaimer -->
        <div class="bg-[#fff2b2] dark:bg-[#3d3003] p-4 rounded-xl text-[#7c620c] dark:text-[#fff2b2]">
            <div class="flex justify-between">
                <p class="font-bold">Disclaimer</p>
            </div>
            <p>
                This generated plate image is for demonstration purposes only and does not represent an actual JPJ
                ePlate. This preview is not affiliated with or endorsed by JPJ Malaysia and should not be used for any
                official purposes.
            </p>
        </div>
    </div>
</template>

<script setup>
import { ref, onMounted } from 'vue'

const plateType = ref('normal')
const plateText = ref('MALAYSIA')
const fontStyle = ref('eu')
const textColor = ref('black')
const isDark = ref(false)

const toggleTheme = () => {
    isDark.value = !isDark.value
    document.documentElement.classList.toggle('dark')
}

onMounted(() => {
    // Check system preference on mount (client-side only)
    if (process.client && window.matchMedia && window.matchMedia('(prefers-color-scheme: dark)').matches) {
        isDark.value = true
        document.documentElement.classList.add('dark')
    }

    // Watch for system theme changes (client-side only)
    if (process.client) {
        window.matchMedia('(prefers-color-scheme: dark)').addEventListener('change', event => {
            isDark.value = event.matches
            document.documentElement.classList.toggle('dark', event.matches)
        })
    }
})

const plateColors = {
    normal: '#0000B8',
    ev: '#8dbf22',
    commercial: '#CC0000',
    military: '#5D6532',
    public: '#FF0000'
}

const textColors = {
    black: "rgb(0, 0, 0)",
    white: "rgb(255, 255, 255)",
    red: "rgb(255, 0, 0)"
}

const fonts = {
    eu: "'EURORegular', sans-serif",
    uk: "'UKNumberPlate', sans-serif",
    old: "'OldMalaysian', sans-serif",
    japan: "'JapanPlate', sans-serif"
}

const backgroundColors = {
    white: "#F4F4F4",
    black: "#000000",
    yellow: "#FFB800"
}

const getDecalColor = (bgColor) => {
    switch (bgColor) {
        case "#0000B8":
            return "Non-EV Cars"
        case "#8dbf22":
            return "EV Cars"
        case "black":
            return "Mafia"
        case "#CC0000":
            return "Commercial Vehicles"
        case "#5D6532":
            return "Army"
        case "#FADA5E":
            return "Royal"
        default:
            return "Unknown"
    }
}
</script>

<style>
@font-face {
    font-family: "EURORegular";
    src: url("//plates5.customeuropeanplates.com/app/resources/fonts/eurov5.woff2") format("woff2");
    font-weight: normal;
    font-style: normal;
}

@font-face {
    font-family: "UKNumberPlate";
    src: url("//fonts.cdnfonts.com/css/uk-number-plate") format("woff2");
    font-weight: normal;
    font-style: normal;
}

@font-face {
    font-family: "OldMalaysian";
    src: url("//fonts.cdnfonts.com/css/din-1451-std") format("woff2");
    font-weight: normal;
    font-style: normal;
}

@font-face {
    font-family: "JapanPlate";
    src: url("//fonts.cdnfonts.com/css/japanese-number-plate") format("woff2");
    font-weight: normal;
    font-style: normal;
}

.euro-font {
    font-family: v-bind('fonts[fontStyle]');
}

.font-montserrat {
    font-family: "Montserrat", sans-serif;
}
</style>