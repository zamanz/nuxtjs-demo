<template>
    <div class="text-center ma-2">
        <v-snackbar v-model="show" :color="color" :timeout="timeout">
            {{ message }}
            <template #action="{ attrs }">
                <button
                    type="button"
                    color="pink"
                    text
                    v-bind="attrs"
                    class="btn me-1 text-danger"
                    @click="show = false"
                >
                    Close
                </button>
            </template>
        </v-snackbar>
    </div>
</template>

<script>
export default {
    data() {
        return {
            show: false,
            message: "",
            color: "",
            timeout: 1500,
        };
    },
    created() {
        this.$store.subscribe((mutation, state) => {
            if (mutation.type === "snackbar/setSnack") {
                this.message = state.snackbar.snack;
                this.color = state.snackbar.color;
                this.show = true;
            }
        });
    }
};
</script>
