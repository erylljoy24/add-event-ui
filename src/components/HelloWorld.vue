<template>

  <div class="container">
    <div class="row">
      <div class="col-md-4">
        
        <div>
          <form v-on:submit.prevent="addEvent">
            <label for="calendar" class="form-label">Event</label>
            <div>
              <input id="calendar" type="text" class="input-group" v-model="form.event_name">
            </div>
            <div class="row form-margin">
              <div class="col-md-6">
                <label for="start-date" class="form-label">From</label>
                <div>
                  <input id="start-date" type="date" class="input-group" v-model="form.start_date">
                </div>
              </div>
              <div class="col-md-6">
                <label for="end-date" class="form-label">To</label>
                <div>
                  <input id="end-date" type="date" class="input-group" v-model="form.end_date">
                </div>
              </div>
            </div>
            <div class="form-margin">
              <label class="checkbox-inline" style="padding-left: 0;">
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
            <div class="row">
              <div class="col-md-6">
                <button type="submit" class="btn btn-success form-margin">Save</button>
              </div>
              <div class="col-md-6">
                <button type="button" @click="viewAllSchedule" class="btn btn-success form-margin">View all</button>
              </div>
            </div>
            
            <div style="margin-top:20px">
              <h4>Select event to view</h4>
              <div v-for="events_btn in events_btns" :key="events_btn.title">
                <button type="button" @click="selectEvent(events_btn.title, events_btn.start, events_btn.end, events_btn.categoryId)" class="btn btn-success form-margin">
                  {{ events_btn.title }}
                </button>
              </div>
            </div>
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
              events: [],
              events_btns: [],
              form: {
                event_name: '',
                start_date: '',
                end_date: '',
                days_selected: []
              }
          }
      },
      mounted () {
        axios
        .get('https://add-event-laravel.herokuapp.com/api/events/all')
        .then((response) => {
          this.events = response.data.events.map((item) => {
            return {
              title: item.event_name,
              start: item.start_date,
              end: item.end_date,
              categoryId: item.categoryId
            }
          })

          this.events_btns = response.data.events.map((item) => {
            return {
              title: item.event_name,
              start: item.start_date,
              end: item.end_date,
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
            
            axios.post('https://add-event-laravel.herokuapp.com/api/event/add', this.form)
              .then((response) => {
                console.log(response)
                this.events = response.data.events.map((item) => {
                  return {
                    title: item.event_name,
                    start: item.start_date,
                    end: item.end_date,
                    categoryId: item.categoryId
                  }
                })
                
                this.events_btns.push({
                  title: response.data.single_event.event_name,
                  start: response.data.single_event.start_date,
                  end: response.data.single_event.end_date,
                  categoryId: 1
                })
              })
              .catch(error => {
                console.log(error)
              })
          },
          selectEvent(event_name, start_date, end_date, categoryId){
             this.events = [
                {
                  title: event_name,
                  start: start_date,
                  end: end_date,
                  categoryId: categoryId
                }
             ]
          },
          viewAllSchedule(){
             this.events = this.events_btns
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
