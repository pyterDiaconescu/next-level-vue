<template>
  <div>
    <h1>Create an Event</h1>
    <form @submit.prevent="createEvent">
      <base-select
        :options="categories"
        label="Add a category"
        v-model="event.category"
        class="field"
        @blur="$v.event.category.$touch()"
        :class="{error: $v.event.category.$error}"
      />
      <template v-if="$v.event.category.$error">
        <p v-if="!$v.event.category.required" class="errorMessage">Category is required</p>
      </template>

      <h3>Name & describe your event</h3>

      <base-input
        label="Title"
        v-model="event.title"
        type="text"
        placeholder="Title"
        class="field"
        @blur="$v.event.title.$touch()"
        :class="{error: $v.event.title.$error}"
      />
      <template v-if="$v.event.title.$error">
        <p v-if="!$v.event.title.required" class="errorMessage">Title is required</p>
      </template>

      <base-input
        label="Description"
        v-model="event.description"
        type="text"
        placeholder="Add a description"
        class="field"
        @blur="$v.event.description.$touch()"
        :class="{error: $v.event.description.$error}"
      />
      <template v-if="$v.event.description.$error">
        <p v-if="!$v.event.description.required" class="errorMessage">Description is required</p>
      </template>

      <h3>Where is your event?</h3>
      <base-input
        label="Location"
        v-model="event.location"
        type="text"
        placeholder="Add a location"
        class="field"
        @blur="$v.event.location.$touch()"
        :class="{error: $v.event.location.$error}"
      />
      <template v-if="$v.event.location.$error">
        <p v-if="!$v.event.location.required" class="errorMessage">Location is required</p>
      </template>
      <h3>When is your event?</h3>

      <div class="field">
        <label>Date</label>
        <datepicker
          v-model="event.date"
          placeholder="Select a date"
          :input-class="{error: $v.event.date.$error}"
          @opened="$v.event.date.$touch()"
        />
      </div>
      <template v-if="$v.event.date.$error">
        <p v-if="!$v.event.date.required" class="errorMessage">Date is required</p>
      </template>
      
      <base-select
        :options="times"
        label="Select a time"
        v-model="event.time"
        class="field"
        @blur="$v.event.time.$touch()"
        :class="{error: $v.event.time.$error}"
      />
      <template v-if="$v.event.time.$error">
        <p v-if="!$v.event.time.required" class="errorMessage">Time is required</p>
      </template>
      <!--
      <input type="submit" class="button -fill-gradient" value="Submit"/>
      -->
      <base-button buttonClass="-fill-gradient">Submit</base-button>
    </form>
  </div>
</template>


<script>
import Datepicker from 'vuejs-datepicker'
import NProgress from 'nprogress'
import { required } from 'vuelidate/lib/validators'
import BaseInput from '@/components/BaseInput'
import BaseSelect from '@/components/BaseSelect'
import BaseButton from '@/components/BaseButton'

export default {
  components: {
    BaseSelect,
    BaseInput,
    Datepicker,
    BaseButton
  },
  data() {
    const times = []
    for (let i = 1; i <= 24; i++) {
      times.push(i + ':00')
    }
    return {
      times,
      categories: this.$store.state.categories,
      event: this.createFreshEventObject()
    }
  },
  validations: {
    event: {
      category: { required },
      title: { required },
      description: { required },
      location: { required },
      date: { required },
      time: { required }
    }
  },
  methods: {
    createEvent() {
      NProgress.start()
      this.$store
        .dispatch('event/createEvent', this.event)
        .then(() => {
          this.$router.push({
            name: 'event-show',
            params: { id: this.event.id }
          })
          this.event = this.createFreshEventObject()
        })
        .catch(() => {
          NProgress.done()
        })
    },
    createFreshEventObject() {
      const user = this.$store.state.user.user
      const id = Math.floor(Math.random() * 10000000)

      return {
        id: id,
        user: user,
        category: '',
        organizer: user,
        title: '',
        description: '',
        location: '',
        date: '',
        time: '',
        attendees: []
      }
    }
  }
}
</script>

<style scoped>
.field {
  margin-bottom: 24px;
}
</style>
