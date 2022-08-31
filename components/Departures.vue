<template>
  <div class="flex flex-col">
    <div class="overflow-x-auto md:overflow-x">
      <table class="container mx-auto my-8">
        <thead
          class="
            text-left
            bg-gradient-to-r
            from-white
            to-light-gray
            my-7
            rounded-lg
            p-4
            text-sm
            md:text-lg
            border-0
            mb-8
          "
        >
          <th class="md:pl-20 rounded-l-lg py-4 px-2 mb-8">Departure Time</th>
          <th class="py-4 px-2">City Name</th>
          <th class="py-4 px-2">Code</th>
          <th class="py-4 px-2">Airline</th>
          <th class="py-4 px-2">Gate</th>
          <th class="py-4 px-2 rounded-r-lg pr-2">Status</th>
        </thead>
        <tbody class="rounded">
          <tr
            v-for="(departure, i) in allDepartures"
            @click.prevent="getSelectedDeparture(departure)"
            :key="departure.id"
            :index="i"
            class="
              content-center
              items-center
              row
              h-24
              text-sm
              md:text-2xl
              border-white border-2
              rounded-l-lg rounded-r-lg
              my-7
              text-white
              font-bold
            "
          >
            <td class="md:pl-20 p-4 my-7 px-2">
              {{ departure.scheduledDepartureDateTime.slice(-8, -3) }}
            </td>
            <td class="px-2 text-dark-yellow">
              {{ departure.arrivalAirport.name.replace(' Airport', '')
              }}<br /><span class="text-light-yellow">{{
                departure.arrivalAirport.countryName
              }}</span>
            </td>
            <td class="px-2">
              {{ departure.arrivalAirport.code.toUpperCase() }}
            </td>
            <td class="px-2">{{ departure.airline.name }}</td>
            <td class="px-2 text-dark-yellow" v-if="departure.departureGate">
              <div v-if="typeof departure.departureGate.number === 'string'">
                {{ departure.departureGate.number }}
              </div>
            </td>
            <td class="px-2 text-dark-yellow" v-else>&nbsp;</td>
            <td class="rounded-r-lg px-2">
              <span :class="classes(departure)">{{ departure.status }}</span>
            </td>
          </tr>
        </tbody>
      </table>
    </div>
    <div
      v-if="showForm"
      class="
        container
        mx-auto
        text-left
        bg-gradient-to-r
        from-white
        to-light-gray
        my-7
        rounded-lg
        p-4
        text-sm
        md:text-lg
        border-0
        mb-8
      "
    >
      <form method="POST" ref="scrollToMe">
        <h3 class="text-gray-500 text-2xl text-center my-8">
          Selected Departure
        </h3>
        <div class="flex flex-col md:flex-row md:mb-10">
          <div class="md:w-1/3 w-full flex-col mb-10 md:mb-0">
            <label for="departureTime" class="text-gray flex flex-col font-bold"
              >Departure Time:</label
            >
            <input
              type="text"
              v-model="this.selectedDeparture.departureTime"
              name="departureTime"
              class="rounded w-full md:w-4/5 p-4 border-2 border-gray-500"
            />
          </div>
          <div class="md:w-1/3 w-full flex-col mb-10 md:mb-0">
            <label for="departureTime" class="text-gray flex flex-col font-bold"
              >City Name:</label
            >
            <input
              type="text"
              v-model="this.selectedDeparture.cityName"
              name="cityName"
              class="rounded w-full md:w-4/5 p-4 border-2 border-gray-500"
            />
          </div>
          <div class="md:w-1/3 w-full flex-col mb-10 md:mb-0">
            <label for="departureTime" class="text-gray flex flex-col font-bold"
              >Country Name:</label
            >
            <input
              type="text"
              v-model="this.selectedDeparture.countryName"
              name="countryName"
              class="rounded w-full md:w-4/5 p-4 border-2 border-gray-500"
            />
          </div>
        </div>
        <div class="flex flex-col md:flex-row md:mb-10">
          <div class="md:w-1/3 w-full flex-col mb-10 md:mb-0">
            <label for="departureTime" class="text-gray flex flex-col font-bold"
              >Code:</label
            >
            <input
              type="text"
              v-model="this.selectedDeparture.code"
              name="departureTime"
              class="rounded w-full md:w-4/5 p-4 border-2 border-gray-500"
            />
          </div>
          <div class="md:w-1/3 w-full flex-col mb-10 md:mb-0">
            <label for="departureTime" class="text-gray flex flex-col font-bold"
              >Airline:</label
            >
            <input
              type="text"
              v-model="this.selectedDeparture.airline"
              name="departureTime"
              class="rounded w-full md:w-4/5 p-4 border-2 border-gray-500"
            />
          </div>
          <div class="md:w-1/3 w-full flex-col mb-10 md:mb-0">
            <label for="departureTime" class="text-gray flex flex-col font-bold"
              >Gate:</label
            >
            <input
              type="text"
              v-model="this.selectedDeparture.gateNo"
              name="departureGate"
              class="rounded w-full md:w-4/5 p-4 border-2 border-gray-500"
            />
          </div>
        </div>
        <div class="flex flex-col md:flex-row md:mb-10">
          <div class="md:w-1/3 w-full flex-col mb-10 md:mb-0">
            <label for="departureTime" class="text-gray flex flex-col font-bold"
              >Status:</label
            >
            <input
              type="text"
              v-model="this.selectedDeparture.status"
              name="departureStatus"
              class="rounded w-full md:w-4/5 p-4 border-2 border-gray-500"
            />
          </div>
          <div class="w-1/3 flex-col">
            <br />
            <button
              type="submit"
              class="button-primary bg-gray-500 rounded text-white py-4 px-12"
            >
              Save
            </button>
          </div>
        </div>
      </form>
    </div>
  </div>
</template>

<script>
export default {
  name: 'departures',
  props: {
    allDepartures: {
      typeof: 'array',
    },
  },
  data() {
    return {
      selectedDeparture: {
        departureTime: '',
        cityName: '',
        countryName: '',
        code: '',
        airline: '',
        gateNo: '',
        status: '',
      },
      showForm: false,
    }
  },
  mounted() {
    this.showForm = false
  },
  methods: {
    classes(departure) {
      if (departure.status.includes('Go to Gate')) {
        return 'border-l-8 border-solid border-blue bg-white text-blue rounded-r-full flex text-black rounded-l-sm p-2 px-4 md:w-72'
      } else if (departure.status.includes('Final Call')) {
        return 'border-l-8 border-solid border-green bg-white text-green rounded-r-full flex text-black rounded-l-sm p-2 px-4 md:w-72'
      } else if (departure.status.includes('Departed')) {
        return 'border-l-8 border-solid border-orange bg-white text-orange rounded-r-full flex text-black rounded-l-sm p-2 px-4 md:w-72'
      } else {
        return 'border-l-8 border-solid border-light-yellow bg-white rounded-r-full flex text-black rounded-l-sm p-2 px-4 md:w-72'
      }
    },
    scrollToForm() {
      const el = this.$refs.scrollToMe

      if (el) {
        el.scrollIntoView({ behavior: 'smooth' })
      }
    },
    getSelectedDeparture(departure) {
      this.showForm = true
      this.scrollToForm()
      this.selectedDeparture.departureTime =
        departure.scheduledDepartureDateTime.slice(-8, -3)
      this.selectedDeparture.cityName = departure.arrivalAirport.name
      this.selectedDeparture.countryName = departure.arrivalAirport.countryName
      this.selectedDeparture.code = departure.arrivalAirport.code.toUpperCase()
      this.selectedDeparture.airline = departure.airline.name
      this.selectedDeparture.gateNo = departure.departureGate.number
      this.selectedDeparture.status = departure.status
      console.log('Selected Departure:', this.selectedDeparture)
    },
  },
}
</script>

<style>
tr {
  cursor: pointer;
}
</style>
