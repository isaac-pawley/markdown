<script setup>
import AppLayout from '@/Layouts/AppLayout.vue';
import {marked} from "marked";
import {computed, ref} from "vue";
import DOMPurify from 'dompurify'
import hljs from 'highlight.js/lib/common'
import 'highlight.js/styles/github-dark.css'

marked.setOptions({
    renderer: new marked.Renderer(),
    highlight: function(code, lang) {
        const language = hljs.getLanguage(lang) ? lang : 'plaintext';
        return hljs.highlight(code, { language }).value;
    },
    langPrefix: 'hljs language-', // highlight.js css expects a top-level 'hljs' class.
    pedantic: false,
    gfm: true,
    breaks: true,
    sanitize: false,
    smartypants: false,
    xhtml: false
});

const md = ref('# Hello World\n' +
    '```php\n' +
    'echo \'Hi\';\n' +
    '```')
const html = computed(() => {
    return DOMPurify.sanitize(marked(md.value));
})
</script>

<template>
    <AppLayout title="Dashboard">
        <template #header>
            <h2 class="font-semibold text-xl text-gray-800 leading-tight">
                Dashboard
            </h2>
        </template>

        <div class="py-12">
            <div class="max-w-7xl mx-auto sm:px-6 lg:px-8">
                <div class="bg-white overflow-hidden shadow-xl sm:rounded-lg">
                    <textarea v-model="md"></textarea>
                    <div v-html="html"></div>
                </div>
            </div>
        </div>
    </AppLayout>
</template>
