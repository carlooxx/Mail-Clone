<template>
    <div>
        <input type="checkbox" :checked="allEmailSelected" @click="bulkSelect" :class="[someEmailSelected ? 'partial-check' : '']" />
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
        let bulkSelect = function() {
            if(allEmailSelected.value) {
                emailSelection.clear()
            } else {
                emailSelection.addMultiple(props.emails)
            }
        }

        return {
            allEmailSelected,
            someEmailSelected,
            bulkSelect
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
