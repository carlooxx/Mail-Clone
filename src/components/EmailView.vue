<template>
    <div class="email-display">
        <div>
            <button>Archive</button>
            <button @click="toggleRead">{{ email.read ? 'Mark as Unread (r)' : 'Mark as read (r)' }}</button>
            <button>Newer</button>
            <button>Older</button>
        </div>
        <h2 class="mb-0">Subject: <strong>{{ email.subject }}</strong></h2>
        <div><em>From {{ email.from }} on {{ format(new Date(email.sentAt), 'MMM do yyyy')}}</em></div>
        <div v-html="marked(email.body)" />
    </div>
</template>

<script>
import { format } from 'date-fns'
import { marked } from 'marked'
import axios from 'axios'
import useKeydown from './composables/use-keydown'

    export default {
        props: {
            email: {
                type: Object,
                required: true
            }
        },
        setup(props) {
            let toggleRead = () => {
            let { email } = props
                email.read = !email.read
                axios.put(`http://localhost:3000/emails/${email.id}`, email)
            }
            useKeydown([{key: 'r', fn: () => { toggleRead() }}])
            return {
                format,
                marked,
                toggleRead
            }
        }
    }
</script>

<style lang="scss" scoped>

</style>