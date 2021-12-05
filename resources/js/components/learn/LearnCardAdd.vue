<template>
    <form>
        <v-text-field
            v-model="cardForm.name"
            label="LearnTitle"
            required
            clearable
            @focusin="startEdit"
            @focusout="finishEdit"
            class="mx-auto pt-0"
            width="100%"
            counter
            :rules="nameRules"
        ></v-text-field>

        <v-select
            v-model="cardForm.list_name"
            :items="listNames"
            label="LearnListName"
            required
            clearable
            @focusin="startEdit"
            @focusout="finishEdit"
            class="mx-auto"
            width="100%"
        ></v-select>

        <v-textarea
            v-model="cardForm.content"
            label="LearnContent"
            required
            clearable
            @focusin="startEdit"
            @focusout="finishEdit"
            class="mx-auto"
            width="100%"
            counter
            :rules="contentRules"
        ></v-textarea>
        <v-select
            v-model="cardForm.status"
            :items="items"
            label="LearnStatus"
            required
            clearable
            @focusin="startEdit"
            @focusout="finishEdit"
            class="mx-auto"
            width="100%"
        ></v-select>

        <v-btn
            class="d-flex mx-auto mb-3 px-10"
            @click="addCardToList"
            :class="[
                isEditing || contentExists
                    ? 'cyan red--text text--lighten-5'
                    : 'indigo darken-4 blue--text text--lighten-5',
            ]"
        >
            LearnAdd
        </v-btn>
    </form>
</template>

<script>
export default {
    name: "CardAdd",
    props: {
        listNames: {
            type: Array,
            required: true,
        },
    },
    data() {
        return {
            cardForm: {
                name: "",
                content: "",
                status: "",
                list_name: "",
            },
            isEditing: false,
            items: ["未着手", "学習中", "保留", "完了"],
            taskListName: [],
            nameRules: [
                (text) => text.length <= 50 || "最大文字数は50文字です",
            ],
            contentRules: [
                (text) => text.length <= 1000 || "最大文字数は1000文字です",
            ],
        };
    },
    computed: {
        contentExists() {
            return (
                this.cardForm.name.length > 0 &&
                this.cardForm.content.length > 0 &&
                this.cardForm.list_name.length > 0 &&
                this.cardForm.status.length > 0
            );
        },
    },
    methods: {
        async addCardToList() {
            await this.$store.dispatch("learn/learnCardCreate", this.cardForm);
            this.cardForm.name = "";
            this.cardForm.content = "";
            this.cardForm.status = "";
            this.cardForm.list_name = "";
        },
        startEdit() {
            this.isEditing = true;
        },
        finishEdit() {
            this.isEditing = false;
        },
    },
};
</script>