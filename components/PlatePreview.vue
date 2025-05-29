<template>
    <div
        class="mx-auto py-4 border-x-2 dark:border-gray-800 border-black rounded-none w-full max-w-7xl text-gray-900 dark:text-white">
        <div class="flex sm:flex-row flex-col justify-between items-center gap-4 mb-8 rounded-none">
            <div class="bg-gray-50/30 p-4 border-2 border-black dark:border-black border-l-0 rounded-none">
                <h1 class="font-bold text-2xl sm:text-3xl sm:text-left text-center">
                    Malaysian e-Plate Generator
                </h1>
            </div>
        </div>

        <div
            class="flex items-center bg-white/80 dark:bg-gray-900/80 shadow-sm mb-2 p-0 border-2 border-x-0 border-black dark:border-black rounded-none">
            <!-- File Button with Dropdown -->
            <div class="relative">
                <button @click="showFileDropdown = !showFileDropdown"
                    class="flex items-center gap-2 hover:bg-gray-100 dark:hover:bg-gray-800 px-4 py-2 rounded-none text-gray-700 dark:text-gray-300 transition-colors">
                    <Icon name="ph:file" class="text-lg" />
                    <span class="text-sm">File</span>
                </button>
                <div v-if="showFileDropdown"
                    class="z-10 absolute bg-white dark:bg-gray-800 shadow-lg mt-1 border border-gray-200 dark:border-gray-700 rounded-none min-w-[200px]">
                    <!-- Save As Submenu -->
                    <button @click="newPlate"
                        class="flex items-center gap-2 hover:bg-gray-100 dark:hover:bg-gray-700 px-4 py-2 w-full text-sm text-left">
                        <Icon name="ph:plus" class="text-lg" />
                        <span>New</span>
                    </button>
                    <div class="group relative">
                        <button @click="showSaveAsDropdown = !showSaveAsDropdown"
                            class="flex justify-between items-center hover:bg-gray-100 dark:hover:bg-gray-700 px-4 py-2 w-full text-sm text-left">
                            <div class="flex items-center gap-2">
                                <Icon name="ph:floppy-disk" class="text-lg" />
                                <span>Save as</span>
                            </div>
                            <Icon name="ph:caret-right" class="text-lg" />
                        </button>
                        <div v-if="showSaveAsDropdown"
                            class="top-0 left-full absolute bg-white dark:bg-gray-800 shadow-lg border border-gray-200 dark:border-gray-700 rounded-none min-w-[200px]">
                            <button @click="saveAs('png')"
                                class="flex items-center gap-2 hover:bg-gray-100 dark:hover:bg-gray-700 px-4 py-2 w-full text-sm text-left">
                                <Icon name="ph:image" class="text-lg" />
                                <span>PNG</span>
                            </button>
                            <button @click="saveAs('jpg')"
                                class="flex items-center gap-2 hover:bg-gray-100 dark:hover:bg-gray-700 px-4 py-2 w-full text-sm text-left">
                                <Icon name="ph:image" class="text-lg" />
                                <span>JPG</span>
                            </button>
                            <button @click="saveAs('svg')"
                                class="flex items-center gap-2 hover:bg-gray-100 dark:hover:bg-gray-700 px-4 py-2 w-full text-sm text-left">
                                <Icon name="ph:image" class="text-lg" />
                                <span>SVG</span>
                            </button>
                            <button @click="saveAs('webp')"
                                class="flex items-center gap-2 hover:bg-gray-100 dark:hover:bg-gray-700 px-4 py-2 w-full text-sm text-left">
                                <Icon name="ph:image" class="text-lg" />
                                <span>WebP</span>
                            </button>
                        </div>
                    </div>
                    <button @click="printPlate"
                        class="flex items-center gap-2 hover:bg-gray-100 dark:hover:bg-gray-700 px-4 py-2 w-full text-sm text-left">
                        <Icon name="ph:printer" class="text-lg" />
                        <span>Print</span>
                    </button>
                </div>
            </div>

            <!-- View Button with Dropdown -->
            <div class="relative">
                <button @click="showViewDropdown = !showViewDropdown"
                    class="flex items-center gap-2 hover:bg-gray-100 dark:hover:bg-gray-800 px-4 py-2 rounded-none text-gray-700 dark:text-gray-300 transition-colors">
                    <Icon name="ph:eye" class="text-lg" />
                    <span class="text-sm">View</span>
                </button>
                <div v-if="showViewDropdown"
                    class="z-10 absolute bg-white dark:bg-gray-800 shadow-lg mt-1 border border-gray-200 dark:border-gray-700 rounded-none min-w-[200px]">
                    <button @click="toggleTheme"
                        class="flex items-center gap-2 hover:bg-gray-100 dark:hover:bg-gray-700 px-4 py-2 w-full text-sm text-left">
                        <Icon :name="isDark ? 'ph:moon-fill' : 'ph:sun-fill'" class="text-lg" />
                        <span>{{ isDark ? 'Dark' : 'Light' }} Mode</span>
                    </button>
                    <div class="group relative">
                        <button @click="showLanguageDropdown = !showLanguageDropdown"
                            class="flex justify-between items-center hover:bg-gray-100 dark:hover:bg-gray-700 px-4 py-2 w-full text-sm text-left">
                            <div class="flex items-center gap-2">
                                <Icon name="ph:translate" class="text-lg" />
                                <span>Language</span>
                            </div>
                            <Icon name="ph:caret-right" class="text-lg" />
                        </button>
                        <div v-if="showLanguageDropdown"
                            class="top-0 left-full absolute bg-white dark:bg-gray-800 shadow-lg border border-gray-200 dark:border-gray-700 rounded-none min-w-[200px]">
                            <button @click="setLanguage('en')" 
                                class="flex items-center gap-2 hover:bg-gray-100 dark:hover:bg-gray-700 px-4 py-2 w-full text-sm text-left">
                                <span>English</span>
                            </button>
                            <button @click="setLanguage('ms')"
                                class="flex items-center gap-2 hover:bg-gray-100 dark:hover:bg-gray-700 px-4 py-2 w-full text-sm text-left">
                                <span>Bahasa Melayu</span>
                            </button>
                        </div>
                    </div>
                </div>
            </div>

            <!-- Help Button with Dropdown -->
            <div class="relative">
                <button @click="showHelpDropdown = !showHelpDropdown"
                    class="flex items-center gap-2 hover:bg-gray-100 dark:hover:bg-gray-800 px-4 py-2 rounded-none text-gray-700 dark:text-gray-300 transition-colors">
                    <Icon name="ph:question" class="text-lg" />
                    <span class="text-sm">Help</span>
                </button>
                <div v-if="showHelpDropdown"
                    class="z-10 absolute bg-white dark:bg-gray-800 shadow-lg mt-1 border border-gray-200 dark:border-gray-700 rounded-none min-w-[200px]">
                    <!-- Go To Submenu -->
                    <div class="group relative">
                        <button @click="showGoToDropdown = !showGoToDropdown"
                            class="flex justify-between items-center hover:bg-gray-100 dark:hover:bg-gray-700 px-4 py-2 w-full text-sm text-left">
                            <div class="flex items-center gap-2">
                                <Icon name="ph:link" class="text-lg" />
                                <span>Go To</span>
                            </div>
                            <Icon name="ph:caret-right" class="text-lg" />
                        </button>
                        <div v-if="showGoToDropdown"
                            class="top-0 left-full absolute bg-white dark:bg-gray-800 shadow-lg border border-gray-200 dark:border-gray-700 rounded-none min-w-[200px]">
                            <a href="https://kebalicious.link" target="_blank"
                                class="block flex items-center gap-2 hover:bg-gray-100 dark:hover:bg-gray-700 px-4 py-2 text-sm">
                                <Icon name="ph:globe" class="text-lg" />
                                <span>Kebal</span>
                            </a>
                            <a href="https://kebal.my" target="_blank"
                                class="block flex items-center gap-2 hover:bg-gray-100 dark:hover:bg-gray-700 px-4 py-2 text-sm">
                                <Icon name="ph:globe" class="text-lg" />
                                <span>Kebal</span>
                            </a>
                            <a href="https://split-bill.kebal.my" target="_blank"
                                class="block flex items-center gap-2 hover:bg-gray-100 dark:hover:bg-gray-700 px-4 py-2 text-sm">
                                <Icon name="ph:globe" class="text-lg" />
                                <span>Split Bill</span>
                            </a>
                        </div>
                    </div>
                    <button @click="showAboutModal = true"
                        class="flex items-center gap-2 hover:bg-gray-100 dark:hover:bg-gray-700 px-4 py-2 w-full text-sm text-left">
                        <Icon name="ph:info" class="text-lg" />
                        <span>About</span>
                    </button>
                </div>
            </div>
        </div>

        <!-- Toolbar ala Microsoft Office -->
        <div
            class="flex items-center gap-2 bg-white/80 dark:bg-gray-900/80 shadow-sm mb-8 p-3 border-2 border-x-0 border-black dark:border-black rounded-none">
            <!-- Plate Type Dropdown -->
            <div class="relative">
                <button @click="showTypeDropdown = !showTypeDropdown"
                    class="flex justify-between items-center gap-2 bg-white dark:bg-gray-800 px-2 py-1 border rounded-none w-64"
                    title="Plate Type">
                    <div class="flex items-center gap-2">
                        <Icon :name="plateTypes.find(t => t.key === plateType).icon" class="text-lg" />
                        <span class="hidden sm:inline">{{plateTypes.find(t => t.key === plateType).label}}</span>
                    </div>
                    <svg class="ml-1 w-4 h-4" fill="none" stroke="currentColor" stroke-width="2" viewBox="0 0 24 24">
                        <path stroke-linecap="round" stroke-linejoin="round" d="M19 9l-7 7-7-7" />
                    </svg>
                </button>
                <div v-if="showTypeDropdown"
                    class="z-10 absolute gap-2 grid grid-cols-3 bg-white dark:bg-gray-900 shadow-lg mt-2 p-2 border border-gray-300 dark:border-gray-700 rounded-none w-56">
                    <button v-for="type in plateTypes" :key="type.key"
                        @click="plateType = type.key; showTypeDropdown = false" :title="type.label" :class="[
                            'flex flex-col items-center justify-center p-2 rounded-none transition-colors',
                            plateType === type.key ? 'bg-blue-100 dark:bg-blue-900' : 'hover:bg-gray-100 dark:hover:bg-gray-800'
                        ]">
                        <Icon :name="type.icon" class="mb-1 text-xl" />
                        <span class="text-xs">{{ type.label }}</span>
                    </button>
                </div>
            </div>

            <!-- Plate Number -->
            <input v-model="plateText" type="text" maxlength="10"
                class="bg-white dark:bg-gray-800 ml-2 px-2 py-1 border rounded-none w-96 text-base" title="Plate Number"
                placeholder="Plate No." />

            <!-- Font Style -->
            <select v-model="fontStyle"
                class="bg-white dark:bg-gray-800 ml-2 px-2 py-1 border rounded-none w-48 text-base" title="Font Style">
                <option v-for="option in fontStyleOptions" :key="option.value" :value="option.value">
                    {{ option.label }}
                </option>
            </select>

            <!-- Text Color Dropdown -->
            <div class="relative ml-2">
                <button @click="showTextColorDropdown = !showTextColorDropdown"
                    class="flex items-center gap-2 bg-white dark:bg-gray-800 px-2 py-1 border rounded-none"
                    title="Text Color">
                    <span class="border border-gray-300 rounded-none w-6 h-6"
                        :style="{ backgroundColor: textColors[textColor] }"></span>
                    <svg class="ml-1 w-4 h-4" fill="none" stroke="currentColor" stroke-width="2" viewBox="0 0 24 24">
                        <path stroke-linecap="round" stroke-linejoin="round" d="M19 9l-7 7-7-7" />
                    </svg>
                </button>
                <div v-if="showTextColorDropdown"
                    class="z-10 absolute gap-2 grid grid-cols-3 bg-white dark:bg-gray-900 shadow-lg mt-2 p-2 border border-gray-300 dark:border-gray-700 rounded-none w-40">
                    <button v-for="color in textColorOptions" :key="color.key"
                        @click="textColor = color.key; showTextColorDropdown = false" :title="color.label" :class="[
                            'w-8 h-8 rounded-none border-2 flex items-center justify-center transition-all',
                            textColor === color.key ? 'ring-2 ring-blue-500' : ''
                        ]" :style="{ backgroundColor: textColors[color.key] }">
                    </button>
                </div>
            </div>

            <!-- Plate Background Color Dropdown -->
            <div class="relative ml-2">
                <button @click="showBgColorDropdown = !showBgColorDropdown"
                    class="flex items-center gap-2 bg-white dark:bg-gray-800 px-2 py-1 border rounded-none"
                    title="Plate Background Color">
                    <span class="border border-gray-300 rounded-none w-6 h-6"
                        :style="{ backgroundColor: plateColors[bgColor] }"></span>
                    <svg class="ml-1 w-4 h-4" fill="none" stroke="currentColor" stroke-width="2" viewBox="0 0 24 24">
                        <path stroke-linecap="round" stroke-linejoin="round" d="M19 9l-7 7-7-7" />
                    </svg>
                </button>
                <div v-if="showBgColorDropdown"
                    class="z-10 absolute gap-2 grid grid-cols-3 bg-white dark:bg-gray-900 shadow-lg mt-2 p-2 border border-gray-300 dark:border-gray-700 rounded-none w-40">
                    <button v-for="color in bgColorOptions" :key="color.key"
                        @click="bgColor = color.key; showBgColorDropdown = false" :title="color.label" :class="[
                            'w-8 h-8 rounded-none border-2 flex items-center justify-center transition-all',
                            bgColor === color.key ? 'ring-2 ring-blue-500' : ''
                        ]" :style="{ backgroundColor: plateColors[color.key] }">
                    </button>
                </div>
            </div>

            <!-- MAL/MY Dropdown -->
            <div class="relative ml-2">
                <select v-model="malText" class="bg-white dark:bg-gray-800 px-2 py-1 border rounded" title="MAL/MY">
                    <option v-for="opt in malTextOptions" :key="opt.key" :value="opt.key">{{ opt.label }}</option>
                </select>
            </div>

            <!-- Flag Type Dropdown -->
            <div class="relative ml-2">
                <select v-model="flagType" class="bg-white dark:bg-gray-800 px-2 py-1 border rounded" title="Flag Type">
                    <option v-for="opt in flagTypeOptions" :key="opt.key" :value="opt.key">{{ opt.label }}</option>
                </select>
            </div>

            <!-- Show Flag Line -->
            <div class="flex items-center ml-2">
                <input type="checkbox" id="showFlagLine" v-model="showFlagLine" class="mr-1" />
                <label for="showFlagLine" class="text-xs">Show Line</label>
            </div>
        </div>

        <!-- Plate Preview -->
        <div
            class="bg-gray-50/30 shadow-sm mb-8 sm:mb-12 border-x-0 border-y-2 border-black dark:border-black rounded-none plate-preview">
            <div class="relative m-12 mx-auto p-2 sm:p-8 rounded-none w-full max-w-[800px] h-[120px] sm:h-[176px]">
                <!-- Main plate content -->
                <div class="absolute inset-0 rounded-lg" :style="{ backgroundColor: plateColors[bgColor] }">
                    <!-- QR Code (top right) -->
                    <div class="top-2 right-2 absolute">
                        <img :src="qrCodeUrl" alt="QR Code" class="w-3 h-3" />
                    </div>

                    <!-- Serial Number (top right) -->
                    <div class="top-2 right-6 sm:right-8 absolute text-[6px] text-gray-400 sm:text-[8px]">000000000
                    </div>

                    <!-- "FRONT" text (bottom right) -->
                    <div class="right-2 bottom-2 absolute text-[6px] text-gray-400 sm:text-[8px]">FRONT</div>

                    <div class="flex flex-row h-full">
                        <!-- Decal Area with Malaysian Flag -->
                        <div class="relative pl-1.5 sm:pl-2.5 rounded-s-xl overflow-hidden" :style="{
                            backgroundColor: plateColors[plateType]
                        }">
                            <div
                                class="flex justify-center items-center mx-auto p-2 sm:p-3 w-full h-[55px] sm:h-[80px]">
                                <img :src="flagUrl" alt="flag" class="mx-auto w-16 h-full object-contain" />
                            </div>
                            <hr v-if="showFlagLine" class="mx-auto mt-2 mb-0 pb-0 border-4 border-white w-8">
                            <div
                                class="flex justify-center items-center h-[55px] sm:h-[81px] font-montserrat font-bold text-white text-sm sm:text-xl tracking-wider">
                                {{ malText }}
                            </div>
                        </div>

                        <!-- Secure Strip -->
                        <div
                            class="relative flex justify-center items-center bg-[#dbdbdb] w-[6px] sm:w-[10px] overflow-hidden">
                            <span
                                class="text-[8px] text-gray-400 sm:text-xs text-center rotate-180 [writing-mode:vertical-rl]">
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

        <!-- Disclaimer -->
        <div
            class="bg-amber-50 dark:bg-amber-950 p-3 sm:p-4 border-amber-800 border-y-2 dark:border-amber-200 rounded-none text-amber-800 dark:text-amber-200 text-sm sm:text-base">
            <div class="flex justify-between">
                <p class="font-bold">Disclaimer</p>
            </div>
            <p>
                This generated plate image is for demonstration purposes only and does not represent an actual JPJ
                ePlate. This preview is not affiliated with or endorsed by JPJ Malaysia and should not be used for any
                official purposes.
            </p>
        </div>

        <!-- About Modal -->
        <div v-if="showAboutModal" class="z-50 fixed inset-0 flex justify-center items-center bg-black bg-opacity-50">
            <div class="bg-white dark:bg-gray-800 mx-4 p-6 rounded-none w-full max-w-md">
                <div class="flex justify-between items-center mb-4 pb-4 border-gray-200 dark:border-gray-700 border-b">
                    <h2 class="font-bold text-xl">About</h2>
                    <button @click="showAboutModal = false"
                        class="text-gray-500 hover:text-red-700 dark:hover:text-gray-300 dark:text-gray-400 transition-colors">
                        <Icon name="ph:x" class="text-lg" />
                    </button>
                </div>
                <p class="mb-4 text-justify">
                    This is an app to generate Malaysian license plate.
                    This tool is for demonstration purposes only and is not affiliated with Road Transport Department
                    Malaysia (JPJ).
                </p>
                <p class="mb-4">
                    This app is created by <a href="https://kebal.my" target="_blank"
                        class="text-blue-500 hover:text-blue-600 dark:hover:text-blue-400 transition-colors">Kebal</a>.
                </p>
            </div>
        </div>
    </div>
</template>

<script setup>
import { ref, onMounted, computed } from 'vue'
import html2canvas from 'html2canvas'

const plateType = ref('ice')
const plateText = ref('MALAYSIA')
const fontStyle = ref('eu')
const textColor = ref('black')
const isDark = ref(false)
const showTypeDropdown = ref(false)
const showTextColorDropdown = ref(false)
const showBgColorDropdown = ref(false)
const bgColor = ref('white')
const showFlagLine = ref(true)
const showFileDropdown = ref(false)
const showHelpDropdown = ref(false)
const showGoToDropdown = ref(false)
const showSaveAsDropdown = ref(false)
const showAboutModal = ref(false)
const showViewDropdown = ref(false)
const showLanguageDropdown = ref(false)
const currentLanguage = ref('en')

const malText = ref('MAL')
const malTextOptions = [
    { key: 'MAL', label: 'MAL' },
    { key: 'MY', label: 'MY' }
]

const flagType = ref('origin')
const flagTypeOptions = [
    { key: 'origin', label: 'Color' },
    { key: 'bw', label: 'B&W' }
]

const flagUrl = computed(() =>
    flagType.value === 'origin'
        ? '/images/flag_ms.webp'
        : '/images/flag_bw_1.webp'
)

const toggleTheme = () => {
    isDark.value = !isDark.value
    document.documentElement.classList.toggle('dark')
}


const fontStyleOptions = [
    { value: 'japan1', label: 'Japanese 1' },
    { value: 'japan2', label: 'Japanese 2' },
    { value: 'japan3', label: 'Japanese 3' },
    { value: 'jpj1', label: 'JPJ 1' },
    { value: 'noplato', label: 'No Plato' },
    { value: 'usa', label: 'USA' },
    { value: 'korea', label: 'Korea' },
    { value: 'arial', label: 'Arial' },
    { value: 'uk', label: 'UK' },
    { value: 'euro', label: 'European' }
]

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

    // Close dropdowns when clicking outside
    document.addEventListener('click', (e) => {
        if (!e.target.closest('.relative')) {
            showFileDropdown.value = false
            showHelpDropdown.value = false
            showGoToDropdown.value = false
            showSaveAsDropdown.value = false
            showViewDropdown.value = false
            showLanguageDropdown.value = false
        }
    })
})

const plateColors = {
    ice: '#0000B8',
    ev: '#8dbf22',
    commercial: '#CC0000',
    military: '#5D6532',
    public: '#00FFFF',
    government: '#800080',
    mafia: '#000000',
    royal: '#FADA5E',
    carbon: '#232b2b',
    olive: '#808000',
    militarygreen: '#4B5320',
    camogreen: '#78866b',
    camonavy: '#3b4c6b',
    camolightblue: '#7ec8e3',
    camored: '#b22222',
    camopolice: '#1e90ff',
    gold: '#FFD700',
    white: '#FFFFFF'
}

const textColors = {
    black: "rgb(0, 0, 0)",
    white: "rgb(255, 255, 255)",
    red: "rgb(255, 0, 0)",
    carbon: "#232b2b",
    olive: "#808000",
    militarygreen: "#4B5320",
    camogreen: "#78866b",
    camonavy: "#3b4c6b",
    camolightblue: "#7ec8e3",
    camored: "#b22222",
    camopolice: "#1e90ff",
    gold: "#FFD700"
}

const fonts = {
    japan1: "'Japan1', sans-serif",
    japan2: "'Japan2', sans-serif",
    japan3: "'Japan3', sans-serif",
    jpj1: "'JPJ1', sans-serif",
    noplato: "'NoPlato', sans-serif",
    usa: "'USA', sans-serif",
    korea: "'Korea', sans-serif",
    arial: "'Arial', sans-serif",
    uk: "'UK', sans-serif",
    euro: "'Euro', sans-serif"
}

const plateTypes = [
    { key: 'ice', icon: 'ph:car', label: 'ICE' },
    { key: 'ev', icon: 'ph:lightning', label: 'EV' },
    { key: 'commercial', icon: 'ph:truck', label: 'Commercial' },
    { key: 'military', icon: 'ph:shield', label: 'Military' },
    { key: 'public', icon: 'ph:bus', label: 'Public Transport' },
    { key: 'government', icon: 'ph:buildings', label: 'Government' },
    { key: 'mafia', icon: 'ph:skull', label: 'Mafia' },
    { key: 'royal', icon: 'ph:crown', label: 'Royal' }
]

const textColorOptions = [
    { key: 'black', label: 'Black' },
    { key: 'white', label: 'White' },
    { key: 'red', label: 'Red' },
    { key: 'carbon', label: 'Carbon' },
    { key: 'olive', label: 'Olive' },
    { key: 'militarygreen', label: 'Military Green' },
    { key: 'camogreen', label: 'Camo Green' },
    { key: 'camonavy', label: 'Camo Navy' },
    { key: 'camolightblue', label: 'Camo Light Blue' },
    { key: 'camored', label: 'Camo Red' },
    { key: 'camopolice', label: 'Police Blue' },
    { key: 'gold', label: 'Gold' }
]

const bgColorOptions = [
    { key: 'white', label: 'White' },
    { key: 'ice', label: 'ICE' },
    { key: 'ev', label: 'EV' },
    { key: 'commercial', label: 'Commercial' },
    { key: 'military', label: 'Military' },
    { key: 'public', label: 'Public Transport' },
    { key: 'government', label: 'Government' },
    { key: 'mafia', label: 'Mafia' },
    { key: 'royal', label: 'Royal' },
    { key: 'carbon', label: 'Carbon' },
    { key: 'olive', label: 'Olive' },
    { key: 'militarygreen', label: 'Military Green' },
    { key: 'camogreen', label: 'Camo Green' },
    { key: 'camonavy', label: 'Camo Navy' },
    { key: 'camolightblue', label: 'Camo Light Blue' },
    { key: 'camored', label: 'Camo Red' },
    { key: 'camopolice', label: 'Police Blue' },
    { key: 'gold', label: 'Gold' }
]

// QR Code generation
const qrCodeUrl = computed(() => {
    const url = 'https://kebal.my'
    const size = 100
    return `https://api.qrserver.com/v1/create-qr-code/?size=${size}x${size}&data=${encodeURIComponent(url)}`
})

// Save plate as image
const saveAs = async (format) => {
    showFileDropdown.value = false
    const plateElement = document.querySelector('.plate-preview')
    if (!plateElement) return

    try {
        const canvas = await html2canvas(plateElement)
        const dataUrl = canvas.toDataURL(`image/${format}`)
        const link = document.createElement('a')
        link.download = `plate.${format}`
        link.href = dataUrl
        link.click()
    } catch (error) {
        console.error('Error saving image:', error)
    }
}

// Print plate
const printPlate = () => {
    showFileDropdown.value = false
    window.print()
}

// New plate
const newPlate = () => {
    showFileDropdown.value = false
    plateText.value = ''
    plateType.value = 'ice'
    fontStyle.value = 'euro'
    textColor.value = 'black'
    bgColor.value = 'white'
    malText.value = 'MAL'
    flagType.value = 'origin'
    showFlagLine.value = true
}

const setLanguage = (lang) => {
    currentLanguage.value = lang
    showLanguageDropdown.value = false
    // Here you can add logic to change the language of the application
}
</script>

<style>
@font-face {
    font-family: "Japan1";
    src: url("/fonts/japan1.woff2") format("woff2");
    font-weight: normal;
    font-style: normal;
}

@font-face {
    font-family: "Japan2";
    src: url("/fonts/japan2.woff2") format("woff2");
    font-weight: normal;
    font-style: normal;
}

@font-face {
    font-family: "Japan3";
    src: url("/fonts/japan3.woff2") format("woff2");
    font-weight: normal;
    font-style: normal;
}

@font-face {
    font-family: "JPJ1";
    src: url("/fonts/jpj1.woff2") format("woff2");
    font-weight: normal;
    font-style: normal;
}

@font-face {
    font-family: "NoPlato";
    src: url("/fonts/noplato.woff2") format("woff2");
    font-weight: normal;
    font-style: normal;
}

@font-face {
    font-family: "USA";
    src: url("/fonts/usa.woff2") format("woff2");
    font-weight: normal;
    font-style: normal;
}

@font-face {
    font-family: "Korea";
    src: url("/fonts/korea.woff2") format("woff2");
    font-weight: normal;
    font-style: normal;
}

@font-face {
    font-family: "Korea2";
    src: url("/fonts/korea2.woff2") format("woff2");
    font-weight: normal;
    font-style: normal;
}

@font-face {
    font-family: "Arial";
    src: url("/fonts/arial.woff2") format("woff2");
    font-weight: normal;
    font-style: normal;
}

@font-face {
    font-family: "UK";
    src: url("/fonts/uk.woff2") format("woff2");
    font-weight: normal;
    font-style: normal;
}

@font-face {
    font-family: "Euro";
    src: url("/fonts/euro.woff2") format("woff2");
    font-weight: normal;
    font-style: normal;
}

.euro-font {
    font-family: v-bind('fonts[fontStyle]');
}

.font-montserrat {
    font-family: "Montserrat", sans-serif;
}

.bg-striped {
    background-image: repeating-linear-gradient(45deg,
            #e5e7eb,
            #e5e7eb 10px,
            #d1d5db 10px,
            #d1d5db 20px);
}
</style>