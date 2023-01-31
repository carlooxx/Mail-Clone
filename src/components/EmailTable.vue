<template>
  <table class="mail-table">
    <tbody>
      <tr
        v-for="email in unarchivedEmails"
        :key="email.id"
        :class="['clickable', email.read ? 'read' : '']"
        @click="openEmail(email)"
      >
        <td><input type="checkbox" /></td>
        <td>{{ email.from }}</td>
        <td>
          <p>
            <strong>{{ email.subject }}</strong> - {{ email.body }}
          </p>
        </td>
        <td class="date">
          {{ format(new Date(email.sentAt), "MMM do yyyy") }}
        </td>
        <td><button @click="archiveEmail(email)">Archive</button></td>
      </tr>
    </tbody>
  </table>
  <ModalView v-if="openedEmail" @closeModal="openedEmail = null">
    <EmailView :email="openedEmail"/> 
  </ModalView>
</template>

<script>
import axios from 'axios'
import { format } from 'date-fns'
import { ref } from 'vue';
import EmailView from '@/components/EmailView.vue';
import ModalView from '@/components/ModalView.vue';

export default {
  async setup() {
    let response = await axios.get('http://localhost:3000/emails')
    let emails = ref(response.data)
    let openedEmail = ref(null)
    return {
        format,
        emails,
        openedEmail
    };
  },
  components: {
    EmailView,
    ModalView
  },
  computed: {
    // sortedEmails(){
    //   return this.emails.sort((e1, e2) => {
    //     return e1.sentAt < e2.sentAt ? 1 : -1
    //   })
    // },
    unarchivedEmails(){
      return this.emails.filter(e => !e.archived)
    }
  },
  methods: {
    openEmail(email){
      email.read = true;
      this.updateEmail(email)
      this.openedEmail = email
    },
    archiveEmail(email){
      email.archived = true
      this.updateEmail(email)
    },
    updateEmail(email){
      axios.put(`http://localhost:3000/emails/${email.id}`, email)
    }
  }
};
</script>

<style lang="scss" scoped></style>
