<template>
  <div class="container mx-auto px-4 my-6 w-full md:w-md lg:w-lg xl:w-xl xxl:w-2xl">
    <notification-popup :messageType="messageType" :message="message" @close="closeNotification" :show-notification="showNotification"></notification-popup>
    <div class="text-center text-grey-dark font-semibold text-xl mb-4">
      {{project.name}}
      <p class="text-sm">December 5, 2017 - December 13, 2017</p>
    </div>

    <!-- Add Member Form -->
    <addMemberForm v-if="addMemberFormShown" @close="closeAddMemberForm" resourceType="project" :resource="project" @addMember="addMember"></addMemberForm>

    <div class="h-16 flex flex-row justify-center items-center px-2">
      <span @click="showAddMemberForm" class="bg-white shadow w-8 h-8 rounded-full text-teal hover:cursor-pointer text-center p-2">
        <i class="fas fa-plus"></i>
      </span>
      <a v-for="(member, index) in project.members" :href="'/users/' + member.username" class="pl-2">
        <img :src="generateUrl(member.avatar)" class="rounded-full w-8 h-8 mr-1">
      </a>
    </div>

    <div class="flex flex-row justify-around my-6 py-4 bg-white shadow rounded text-grey">
      <div @click="activateThisTab('tasks')"
        :class="[(active === 'tasks') ? 'text-teal-dark font-semibold border-teal border-b-2 pb-4 -mb-4' : 'cursor-pointer', 'text-center w-1/6']">
        <i class="fas fa-tasks text-2xl"></i>
      </div>
      <div @click="activateThisTab('discussions')"
        :class="[(active === 'discussions') ? 'text-teal-dark font-semibold border-teal border-b-2 pb-4 -mb-4' : 'cursor-pointer', 'text-center w-1/6']">
        <i class="fas fa-clipboard-list text-2xl"></i>
      </div>
      <div @click="activateThisTab('messages')"
        :class="[(active === 'messages') ? 'text-teal-dark font-semibold border-teal border-b-2 pb-4 -mb-4' : 'cursor-pointer', 'text-center w-1/6']">
        <i class="fas fa-comments text-2xl"></i>
      </div>
      <div @click="activateThisTab('events')"
        :class="[(active === 'events') ? 'text-teal-dark font-semibold border-teal border-b-2 pb-4 -mb-4' : 'cursor-pointer', 'text-center w-1/6']">
        <i class="fas fa-calendar-alt text-2xl"></i>
      </div>
      <div @click="activateThisTab('files')"
        :class="[(active === 'files') ? 'text-teal-dark font-semibold border-teal border-b-2 pb-4 -mb-4' : 'cursor-pointer', 'text-center w-1/6']">
        <i class="fas fa-file-alt text-2xl"></i>
      </div>
      <div @click="activateThisTab('activities')"
        :class="[(active === 'activities') ? 'text-teal-dark font-semibold border-teal border-b-2 pb-4 -mb-4' : 'cursor-pointer', 'text-center w-1/6']">
        <i class="fas fa-bolt text-2xl"></i>
      </div>
    </div>

    <div class="flex flex-row flex-wrap justify-start">
      <taskBoard resourceType="project" :resource="project"  :activeTab="active"></taskBoard>
      <discussionBoard resourceType="project" :resource="project" :activeTab="active"></discussionBoard>
      <messagesBoard resourceType="project" :resource="project" :activeTab="active"></messagesBoard>
      <!-- <messagesBoard resourceType="projects" :resource="project"></messagesBoard>
      <schedule resourceType="projects" :resource="project"></schedule>
      <files resourceType="projects" :resource="project"></files>
      <activity resourceType="projects" :resource="project"></activity> -->
    </div>
  </div>
</template>

<script>
import taskBoard from './../partials/taskBoard.vue'
import discussionBoard from './../partials/discussionBoard.vue'
import messagesBoard from './../partials/messagesBoard.vue'
import schedule from './../partials/schedule.vue'
import files from './../partials/files.vue'
import activity from './../partials/activity.vue'
import addMemberForm from './../partials/addMemberForm.vue'
import notificationPopup from '../partials/notificationPopup.vue'

export default {
  components: {
    taskBoard, discussionBoard, messagesBoard, schedule, files, activity, addMemberForm, notificationPopup
  },
  props: ['project'],
  data: () => ({
    addMemberFormShown: false,
    active: 'tasks',
    showNotification: false,
    message: '',
    messageType: '',
  }),
  methods: {
    showAddMemberForm () {
      this.addMemberFormShown = true
    },
    closeAddMemberForm () {
      this.addMemberFormShown = false
    },
    addMember (data) {
      if (data.user) {
        this.message = data.message
        this.messageType = 'success'
        this.project.members.push(data.user)
      } else {
        this.messageType = 'error'
        this.message = data.message
      }
      this.showNotification = true
      this.addMemberFormShown = false
      setTimeout(() => {
        this.showNotification = false
      }, 3000)
    },
    activateThisTab (tab) {
      if (tab != this.active) {
        this.active = tab
      }
    },
    closeNotification () {
      this.showNotification = false
    }
  }
}
</script>
