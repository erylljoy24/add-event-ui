<template>

  <div class="container">
    <div class="row">
      <div class="col-md-4">
        <div>
          <label for="calendar" class="form-label">Event</label>
          <div>
            <input id="calendar" type="text" class="input-group">
          </div>
          <div class="row form-margin">
            <div class="col-md-6">
              <label for="start-date" class="form-label">From</label>
              <div>
                <input id="start-date" type="date" class="input-group" v-model="startDate">
              </div>
            </div>
            <div class="col-md-6">
              <label for="end-date" class="form-label">To</label>
              <div>
                <input id="end-date" type="date" class="input-group" v-model="endDate">
              </div>
            </div>
          </div>
          <div class="form-margin">
            <label class="checkbox-inline">
              <input type="checkbox" id="mon" value="1">Mon
            </label>
            <label class="checkbox-inline">
              <input type="checkbox" id="tue" value="2">Tue
            </label>
            <label class="checkbox-inline">
              <input type="checkbox" id="wed" value="3">Wed
            </label>
            <label class="checkbox-inline">
              <input type="checkbox" id="thu" value="4">Thu
            </label>
            <label class="checkbox-inline">
              <input type="checkbox" id="fri" value="5">Fri
            </label>
            <label class="checkbox-inline">
              <input type="checkbox" id="sat" value="6">Sat
            </label>
            <label class="checkbox-inline">
              <input type="checkbox" id="sun" value="7">Sun
            </label>
          </div>
          <button type="submit" class="btn btn-success form-margin">Save</button>
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
              calendarData: {},
              eventCategories: [
                  {
                      id: 1,
                      title: 'Personal',
                      textColor: 'white',
                      backgroundColor: 'Blue'
                  },
                  {
                      id: 2,
                      title: 'Company-wide',
                      textColor: 'white',
                      backgroundColor: 'red'
                  }
              ],
              events: [
                  {
                      title: 'Event 1',
                      start: '2019-04-02',
                      end: '2019-04-04',
                      repeat: 'monthly',
                      categoryId: 1
                  },
                  {
                      title: 'Event 2',
                      start: '2019-04-08',
                      end: '2019-04-09',
                      repeat: 'yearly',
                      categoryId: 1
                  },
                  {
                      title: 'Event 3',
                      start: '2019-04-10',
                      end: '2019-04-11',
                      repeat: 'never',
                      categoryId: 2
                  },
                  {
                      title: 'Event 4',
                      start: '2019-04-23',
                      end: '2019-04-23',
                      repeat: 'monthly',
                      categoryId: 2
                  }
              ],
          }
      },
      methods: {
          goToday() {
              this.$refs.calendar.goToday()
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
