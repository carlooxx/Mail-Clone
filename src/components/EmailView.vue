<template>
    <div class="email-display">
        <div>
            <button @click="toggleArchive">{{ email.archived ? 'Move to Inbox (e)' : 'Archive (e)' }}</button>
            <button @click="toggleRead">{{ email.read ? 'Mark as Unread (r)' : 'Mark as read (r)' }}</button>
            <button @click="goNewer">Newer (k)</button>
            <button @click="goOlder">Older (j)</button>
        </div>
        <h2 class="mb-0">Subject: <strong>{{ email.subject }}</strong></h2>
        <div><em>From {{ email.from }} on {{ format(new Date(email.sentAt), 'MMM do yyyy')}}</em></div>
        <div v-html="marked(email.body)" />
    </div>
</template>

<script>
import { format } from 'date-fns'
import { marked } from 'marked'
import useKeydown from './composables/use-keydown'

export default {
    props: {
        email: {
            type: Object,
            required: true
        }
    },
    setup(props,{ emit }) {
        let toggleRead = () => { emit('changeEmail', { toggleRead: true, save: true }) }
        let toggleArchive = () => { emit('changeEmail', { toggleArchive: true, save: true, closeModal: true }) }
        let goNewer = () => { emit('changeEmail', { changeIndex: -1 }) }
        let goOlder = () => { emit('changeEmail', { changeIndex: 1 }) }
        let goNewerAndArchived = () => { emit('changeEmail', { changeIndex: 1, toggleArchive: true, save: true }) }
        let goOlderAndArchived = () => { emit('changeEmail', { changeIndex: 1, toggleArchive: true, save: true }) }

        useKeydown([
            { key: 'r', fn: () => { toggleRead() } },
            { key: 'e', fn: () => { toggleArchive() } },
            { key: 'k', fn: () => { goNewer() } },
            { key: 'j', fn: () => { goOlder() } },
            { key: '[', fn: () => { goNewerAndArchived() } },
            { key: ']', fn: () => { goOlderAndArchived() } },
        ])
        return {
            format,
            marked,
            toggleRead,
            toggleArchive,
            goNewer,
            goOlder
        }
    }
}
</script>

<style lang="scss" scoped>

</style>