<template>
    <div class="mx-auto px-4 sm:px-8 md:px-16 py-4 w-full max-w-7xl text-gray-900 dark:text-white">
        <div class="flex sm:flex-row flex-col justify-between items-center gap-4 mb-8">
            <h1 class="font-bold text-2xl sm:text-3xl sm:text-left text-center">
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
        <div class="bg-white dark:bg-gray-900 shadow-sm mb-8 sm:mb-12 border-4 border-gray-100 dark:border-gray-800 rounded-xl rounded-t-lg">
            <h2 class="bg-gray-100 dark:bg-gray-800 mb-4 p-2 overflow-hidden font-semibold text-xl">Preview</h2>
            <div class="relative mx-auto p-2 sm:p-4 w-full max-w-[800px] h-[120px] sm:h-[176px]">
                <!-- Main plate content -->
                <div class="absolute inset-0 bg-[#F4F4F4] rounded-lg">
                    <!-- QR Code (top right) -->
                    <div class="top-2 right-2 absolute bg-black rounded-sm w-3 sm:w-4 h-3 sm:h-4"></div>

                    <!-- Serial Number (top right) -->
                    <div class="top-2 right-6 sm:right-8 absolute text-[6px] text-gray-400 sm:text-[8px]">000007543</div>

                    <!-- "FRONT" text (bottom right) -->
                    <div class="right-2 bottom-2 absolute text-[6px] text-gray-400 sm:text-[8px]">FRONT</div>

                    <div class="flex flex-row h-full">
                        <!-- Decal Area with Malaysian Flag -->
                        <div class="relative pl-1.5 sm:pl-2.5 rounded-s-xl overflow-hidden" :style="{
                            backgroundColor: plateColors[plateType]
                        }">
                            <div class="flex justify-center items-center mx-auto p-2 sm:p-3 w-full h-[55px] sm:h-[80px]">
                                <img src="https://i.imgur.com/MPgmBgw.png" alt="flag"
                                    class="w-full h-full object-contain" />
                            </div>
                            <div
                                class="flex justify-center items-center h-[55px] sm:h-[81px] font-montserrat font-bold text-white text-sm sm:text-xl tracking-wider">
                                MAL
                            </div>
                        </div>

                        <!-- Secure Strip -->
                        <div class="relative flex justify-center items-center bg-[#dbdbdb] w-[6px] sm:w-[10px] overflow-hidden">
                            <span class="text-[8px] text-gray-400 sm:text-xs text-center rotate-180 [writing-mode:vertical-rl]">
                                SECURE SECURE SECURE
                            </span>
                        </div>

                        <!-- Plate Text Area -->
                        <div class="flex flex-1 justify-center items-center">
                            <div class="text-5xl sm:text-8xl euro-font" :style="{
                                color: textColors[textColor],
                                letterSpacing: '2px sm:4px'
                            }">
                                {{ plateText }}
                            </div>
                        </div>
                    </div>
                </div>

                <!-- Border effect overlay -->
                <div class="absolute inset-0 rounded-lg pointer-events-none" :style="{
                    boxShadow: `
                            inset 0 0 0 4px sm:8px #e2e2e2,
                            inset 2px 2px 4px rgba(0, 0, 0, 0.1),
                            inset -1px -1px 3px rgba(255, 255, 255, 0.3)
                        `
                }"></div>
            </div>
        </div>

        <!-- Controls -->
        <div class="bg-white dark:bg-gray-900 shadow-sm mb-8 sm:mb-12 border-4 border-gray-100 dark:border-gray-800 rounded-xl rounded-t-lg">
            <h2 class="bg-gray-100 dark:bg-gray-800 mb-4 p-2 overflow-hidden font-semibold text-xl">Controls</h2>
            <div class="gap-4 sm:gap-6 grid grid-cols-1 md:grid-cols-2 p-3 sm:p-4">
                <div>
                    <label class="block mb-2 text-base sm:text-lg">Plate Type</label>
                    <div class="flex flex-wrap gap-2">
                        <button @click="plateType = 'ice'" :class="[
                            'px-3 sm:px-4 py-1.5 sm:py-2 rounded-lg text-xs sm:text-sm transition-colors',
                            plateType === 'ice' ? 'bg-blue-600 text-white' : 'bg-gray-100 dark:bg-gray-800 hover:bg-gray-200 dark:hover:bg-gray-700'
                        ]">
                            ICE Car
                        </button>
                        <button @click="plateType = 'ev'" :class="[
                            'px-3 sm:px-4 py-1.5 sm:py-2 rounded-lg text-xs sm:text-sm transition-colors',
                            plateType === 'ev' ? 'bg-green-600 text-white' : 'bg-gray-100 dark:bg-gray-800 hover:bg-gray-200 dark:hover:bg-gray-700'
                        ]">
                            EV
                        </button>
                        <button @click="plateType = 'commercial'" :class="[
                            'px-3 sm:px-4 py-1.5 sm:py-2 rounded-lg text-xs sm:text-sm transition-colors',
                            plateType === 'commercial' ? 'bg-yellow-600 text-white' : 'bg-gray-100 dark:bg-gray-800 hover:bg-gray-200 dark:hover:bg-gray-700'
                        ]">
                            Commercial
                        </button>
                        <button @click="plateType = 'military'" :class="[
                            'px-3 sm:px-4 py-1.5 sm:py-2 rounded-lg text-xs sm:text-sm transition-colors',
                            plateType === 'military' ? 'bg-green-800 text-white' : 'bg-gray-100 dark:bg-gray-800 hover:bg-gray-200 dark:hover:bg-gray-700'
                        ]">
                            Military
                        </button>
                        <button @click="plateType = 'public'" :class="[
                            'px-3 sm:px-4 py-1.5 sm:py-2 rounded-lg text-xs sm:text-sm transition-colors',
                            plateType === 'public' ? 'bg-red-600 text-white' : 'bg-gray-100 dark:bg-gray-800 hover:bg-gray-200 dark:hover:bg-gray-700'
                        ]">
                            Public Transport
                        </button>
                        <button @click="plateType = 'mafia'" :class="[
                            'px-3 sm:px-4 py-1.5 sm:py-2 rounded-lg text-xs sm:text-sm transition-colors',
                            plateType === 'mafia' ? 'bg-black text-white' : 'bg-gray-100 dark:bg-gray-800 hover:bg-gray-200 dark:hover:bg-gray-700'
                        ]">
                            Mafia
                        </button>
                        <button @click="plateType = 'royal'" :class="[
                            'px-3 sm:px-4 py-1.5 sm:py-2 rounded-lg text-xs sm:text-sm transition-colors',
                            plateType === 'royal' ? 'bg-yellow-500 text-white' : 'bg-gray-100 dark:bg-gray-800 hover:bg-gray-200 dark:hover:bg-gray-700'
                        ]">
                            Royal
                        </button>
                    </div>
                </div>

                <div>
                    <label class="block mb-2 text-base sm:text-lg">Plate Number</label>
                    <input v-model="plateText" type="text" maxlength="10"
                        class="bg-white dark:bg-gray-800 p-3 sm:p-4 border border-gray-200 dark:border-gray-700 rounded-xl w-full text-lg sm:text-xl uppercase" />
                </div>

                <div>
                    <label class="block mb-2 text-base sm:text-lg">Font Style</label>
                    <div class="flex flex-wrap gap-2">
                        <button @click="fontStyle = 'eu'" :class="[
                            'px-3 sm:px-4 py-1.5 sm:py-2 rounded-lg text-xs sm:text-sm transition-colors',
                            fontStyle === 'eu' ? 'bg-blue-600 text-white' : 'bg-gray-100 dark:bg-gray-800 hover:bg-gray-200 dark:hover:bg-gray-700'
                        ]">
                            European
                        </button>
                        <button @click="fontStyle = 'uk'" :class="[
                            'px-3 sm:px-4 py-1.5 sm:py-2 rounded-lg text-xs sm:text-sm transition-colors',
                            fontStyle === 'uk' ? 'bg-blue-600 text-white' : 'bg-gray-100 dark:bg-gray-800 hover:bg-gray-200 dark:hover:bg-gray-700'
                        ]">
                            UK/Singapore
                        </button>
                        <button @click="fontStyle = 'old'" :class="[
                            'px-3 sm:px-4 py-1.5 sm:py-2 rounded-lg text-xs sm:text-sm transition-colors',
                            fontStyle === 'old' ? 'bg-blue-600 text-white' : 'bg-gray-100 dark:bg-gray-800 hover:bg-gray-200 dark:hover:bg-gray-700'
                        ]">
                            Old Malaysian
                        </button>
                        <button @click="fontStyle = 'japan'" :class="[
                            'px-3 sm:px-4 py-1.5 sm:py-2 rounded-lg text-xs sm:text-sm transition-colors',
                            fontStyle === 'japan' ? 'bg-blue-600 text-white' : 'bg-gray-100 dark:bg-gray-800 hover:bg-gray-200 dark:hover:bg-gray-700'
                        ]">
                            Japanese
                        </button>
                    </div>
                </div>

                <div>
                    <label class="block mb-2 text-base sm:text-lg">Text Color</label>
                    <div class="flex flex-wrap gap-2">
                        <button 
                            @click="textColor = 'black'" 
                            class="rounded-lg ring-2 ring-offset-2 w-8 sm:w-10 h-8 sm:h-10 hover:scale-110 transition-transform"
                            :class="[
                                'bg-black',
                                textColor === 'black' ? 'ring-blue-500' : 'ring-transparent'
                            ]"
                        >
                        </button>
                        <button 
                            @click="textColor = 'white'" 
                            class="rounded-lg ring-2 ring-offset-2 w-8 sm:w-10 h-8 sm:h-10 hover:scale-110 transition-transform"
                            :class="[
                                'bg-white',
                                textColor === 'white' ? 'ring-blue-500' : 'ring-transparent'
                            ]"
                        >
                        </button>
                        <button 
                            @click="textColor = 'red'" 
                            class="rounded-lg ring-2 ring-offset-2 w-8 sm:w-10 h-8 sm:h-10 hover:scale-110 transition-transform"
                            :class="[
                                'bg-red-600',
                                textColor === 'red' ? 'ring-blue-500' : 'ring-transparent'
                            ]"
                        >
                        </button>
                    </div>
                </div>
            </div>
        </div>

        <!-- Disclaimer -->
        <div class="bg-amber-50 dark:bg-amber-950 p-3 sm:p-4 rounded-xl text-amber-800 dark:text-amber-200 text-sm sm:text-base">
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

const plateType = ref('ice')
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
    ice: '#0000B8',
    ev: '#8dbf22',
    commercial: '#CC0000',
    military: '#5D6532',
    public: '#FF0000',
    mafia: '#000000',
    royal: '#FADA5E'
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