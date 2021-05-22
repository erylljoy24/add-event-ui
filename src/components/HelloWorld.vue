<template>

  <div class="container">
    <div class="row">
      <div class="col-md-4">
        
        <div>
          <form v-on:submit.prevent="addEvent">
            <label for="calendar" class="form-label">Event</label>
            <div>
              <input id="calendar" type="text" class="input-group" v-model="form.title">
            </div>
            <div class="row form-margin">
              <div class="col-md-6">
                <label for="start-date" class="form-label">From</label>
                <div>
                  <input id="start-date" type="date" class="input-group" v-model="form.start">
                </div>
              </div>
              <div class="col-md-6">
                <label for="end-date" class="form-label">To</label>
                <div>
                  <input id="end-date" type="date" class="input-group" v-model="form.end">
                </div>
              </div>
            </div>
            <div class="form-margin">
              <label class="checkbox-inline">
                <input type="checkbox" id="mon" v-model="form.days_selected" :value="1">Mon
              </label>
              <label class="checkbox-inline">
                <input type="checkbox" id="tue" v-model="form.days_selected" :value="2">Tue
              </label>
              <label class="checkbox-inline">
                <input type="checkbox" id="wed" v-model="form.days_selected" :value="3">Wed
              </label>
              <label class="checkbox-inline">
                <input type="checkbox" id="thu" v-model="form.days_selected" :value="4">Thu
              </label>
              <label class="checkbox-inline">
                <input type="checkbox" id="fri" v-model="form.days_selected" :value="5">Fri
              </label>
              <label class="checkbox-inline">
                <input type="checkbox" id="sat" v-model="form.days_selected" :value="6">Sat
              </label>
              <label class="checkbox-inline">
                <input type="checkbox" id="sun" v-model="form.days_selected" :value="7">Sun
              </label>
            </div>
            <button type="submit" class="btn btn-success form-margin">Save</button>
          </form>
        </div>
      </div>
      <div class="col-md-8">
        <calendar
          :eventCategories="eventCategories"
          :events="events"
          ref="calendar"
        />
      </div>
    </div>
  </div>
</template>

<script>
    // import Datepicker from 'vuejs-datepicker';
    import { Calendar } from 'vue-sweet-calendar'
    import 'vue-sweet-calendar/dist/SweetCalendar.css'
    import axios from 'axios';

    export default {
      name: 'HelloWorld',
      props: {
        msg: String
      },
      components: {
          // Datepicker
          // FunctionalCalendar
          Calendar
      },
      data() {
          return {
              eventCategories: [
                  {
                      id: 1,
                      title: 'Personal',
                      textColor: 'white',
                      backgroundColor: 'Blue'
                  }
              ],
              events: [
              ],


              form: {
                title: '',
                start: '',
                end: '',
                days_selected: []
              }
          }
      },
      mounted () {
        axios
        .get('http://127.0.0.1:8000/api/events/all')
        .then((response) => {
          this.events = response.data.events.map((item) => {
            return {
              title: item.title,
              start: item.start,
              end: item.end,
              categoryId: item.categoryId
            }
          })
        })
      },
      methods: {
          goToday() {
              this.$refs.calendar.goToday()
          },

          addEvent(){
            console.log(this.form)
            axios.post('http://127.0.0.1:8000/api/event/add', this.form)
              .then((response) => {
                this.events = response.data.events.map((item) => {
                  return {
                    title: item.title,
                    start: item.start,
                    end: item.end,
                    categoryId: item.categoryId
                  }
                })
              })
              .catch(error => {
                console.log(error)
              })
          }
      }
    }
</script>
<style scoped>
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
.form-margin {
  margin-top: 10px;
}
.checkbox-inline {
  position: relative;
  display: inline-block;
  padding-left: 20px;
  margin-bottom: 0;
  font-weight: 400;
  vertical-align: middle;
  cursor: pointer;
}
</style>
