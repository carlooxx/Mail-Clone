<template>
    <div class="bulk-action-bar">
        <span class="checkbox">
            <input type="checkbox" :checked="allEmailSelected" @click="bulkSelect"
                :class="[someEmailSelected ? 'partial-check' : '']" />
        </span>
        <button @click="emailSelection.markRead()" :disabled="[...emailSelection.emails].every(e => e.read)">Mark read</button>
        <button @click="emailSelection.markUnread()" :disabled="[...emailSelection.emails].every(e => !e.read)">Mark unread</button>
        <button @click="emailSelection.archive()" :disabled="numberSelected === 0">Archive</button>
    </div>
</template>

<script>
import useEmailSelection from "./composables/useEmailSelection";
import { computed, toRefs } from "vue";

export default {
    setup(props) {
        let { emails } = toRefs(props);
        let emailSelection = useEmailSelection();
        let numberSelected = computed(() => emailSelection.emails.size);
        let numberEmails = emails.value.length;
        let allEmailSelected = computed(
            () => numberSelected.value === numberEmails
        );
        let someEmailSelected = computed(
            () => numberSelected.value > 0 && numberSelected.value < numberEmails
        );
        let bulkSelect = function () {
            if (allEmailSelected.value) {
                emailSelection.clear()
            } else {
                emailSelection.addMultiple(props.emails)
            }
        }

        return {
            allEmailSelected,
            someEmailSelected,
            bulkSelect,
            emailSelection,
            numberSelected
        };
    },
    props: {
        emails: {
            type: Array,
            required: true,
        },
    },
};
</script>

<style lang="scss" scoped>

</style>
