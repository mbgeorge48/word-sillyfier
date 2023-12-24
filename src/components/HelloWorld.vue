<script setup lang="ts">
import { ref, watch } from "vue";
defineProps<{ msg: string }>();

const sentence = ref("");
const emoji = ref();
const emojiCustom = ref();

let transformedText = "";
function transformText() {
    if (emoji.value === "alt-case") {
        transformedText = sentence.value
            .toLowerCase()
            .split("")
            .map((letter, index) => {
                return index % 2 == 0 ? letter.toUpperCase() : letter;
            })
            .join("");
    } else if (emoji.value === "custom") {
        transformedText = sentence.value
            .trim()
            .replaceAll(/ +(?= )/g, "")
            .replaceAll(" ", emojiCustom.value);
    } else {
        transformedText = sentence.value
            .trim()
            .replaceAll(/ +(?= )/g, "")
            .replaceAll(" ", emoji.value);
    }
}
watch(emoji, transformText, { immediate: true });
watch(emojiCustom, transformText, { immediate: true });
</script>

<template>
    <div class="card">
        <h1>{{ msg }}</h1>
        <div class="input-set">
            <label for="emoji">Select your emoji</label>
            <select v-model="emoji">
                <option disabled value="">Please select one</option>
                <option value="👏">👏 Clapping hands</option>
                <option value="🤡">🤡 Clown</option>
                <option value="😎">😎 Sunglasses</option>
                <option value="alt-case">aLt cAsE</option>
                <option value="custom">Custom</option>
            </select>
            <div v-if="emoji === 'custom'">
                <label for="emoj-custom">Enter your custom divider</label>
                <input
                    class="input"
                    v-model="emojiCustom"
                    id="emoji-custom"
                    @input="transformText"
                />
            </div>
        </div>
        <div class="input-set">
            <label for="sentence">Enter your sentence</label>
            <textarea
                class="input"
                v-model="sentence"
                id="sentence"
                @input="transformText"
                rows="5"
                cols="30"
            />
        </div>
        <p>
            {{ transformedText }}
        </p>
    </div>
    <div class="background" v-if="emoji !== 'alt-case' && emoji !== 'custom'">
        <span v-for="_ in 100">{{ emoji }} </span>
    </div>
</template>
