<script>
import { discs } from "./data/discs.json";

export default {
  name: 'App',
  data(){ 
    return {
      randomBag: [],
      selectedManufacturers: [],
      rimWidths: [],
      speeds: [],
      turns: [],
      glides: [],
      fades: [],
      plasticTypes: [],
    }
  },
  computed: {
    discCount(){ 
      return discs.length;
    },
    manufacturersArray(){
      const manufacturerDuplicates = discs.map(disc => disc.Manufacturer);
      const manufacturerList = [];
      for (const i in manufacturerDuplicates) {
        //manufacturerList.push(manufacturerDuplicates[i].replace(/-/g," "));
        manufacturerList.push(manufacturerDuplicates[i]);
      }
      return [...new Set(manufacturerList)];
    },
    maufacturers() {
      return this.manufacturersArray.map(i => Object.create({ name: i }));
    },  
    discsForSelectedManufacturers() {
      return discs.filter(disc => this.selectedManufacturers.includes(disc.Manufacturer));
    },
    selectedItem() {
      if (this.isSelected.map(item => item)) {
        return true;
      } else {
        return false;
      }
    }
  },
  methods: {
    toggleSelect(manufacturer) {
        for (const i in this.selectedManufacturers) {
          if (this.selectedManufacturers[i] === manufacturer) {
          this.selectedManufacturers = this.selectedManufacturers.filter(man => man != this.selectedManufacturers[i]);
          return;
        }
      }
      this.selectedManufacturers.push(manufacturer);
      console.log(this.selectedManufacturers)
    },
    filterDiscTypes() {
      let putters = [];
      let midranges = [];
      let drivers = [];
      this.discsForSelectedManufacturers.forEach(disc => { 
        if (disc.Speed <= 3) {
          putters.push(disc);
        } else if (disc.Speed > 3 && disc.Speed <= 6) {
          midranges.push(disc);
        } else if (disc.Speed > 6) {
          drivers.push(disc);
        }
      })
      putters = putters.sort(() => 0.5 - Math.random());
      putters = putters.slice(0, this.putterCount);
      midranges = midranges.sort(() => 0.5 - Math.random());
      midranges = midranges.slice(0, this.midrangeCount);
      drivers = drivers.sort(() => 0.5 - Math.random());
      drivers = drivers.slice(0, this.driverCount);

      //this.randomBag = [...putters, ...midranges, ...drivers];
      this.randomBag = {
        putters,
        midranges,
        drivers
      }
    },
    generateBag() {
      console.log('Generating bag...')
      this.filterDiscTypes();
      const bag = {
        putters: [],
        midranges: [],
        drivers: [],
      }
    }
  },
}
</script>

<template>
  <div class="m-24">
    <div class="pb-12">
      <p class="text-4xl font-thin">Select your favorite brands to get started:</p>
      <p class="mt-5">You have selected: <span v-if="this.selectedManufacturers.length > 0">{{ this.selectedManufacturers }}</span></p>
    </div>
    <div class="grid grid-cols-9 gap-4">
      <ul v-for="manufacturer in manufacturersArray" :key="manufacturer" @click="toggleSelect(manufacturer)" class="rounded bg-blue-500 hover:bg-blue-800 transition-colors cursor-pointer">
        <div class="space-between m-auto text-white m-5">
              <li class="grid flex">{{ manufacturer }}</li>
        </div>
      </ul>
    </div>
    <form action="">
      <div>
        <div class="grid grid-cols-4 gap-4 pt-12">
          <div>
            <label class="block mb-2 text-sm font-medium text-gray-900">How many putt and approach discs?</label>
            <select v-model="putterCount" class="bg-gray-50 border border-gray-300 text-gray-900 text-sm rounded-lg focus:ring-blue-500 focus:border-blue-500 block w-full p-2.5 dark:bg-gray-700 dark:border-gray-600 dark:placeholder-gray-400 dark:text-white dark:focus:ring-blue-500 dark:focus:border-blue-500">
              <option value="0">0</option>
              <option value="1">1</option>
              <option value="2">2</option>
              <option value="3">3</option>
              <option value="4">4</option>
              <option value="5">5</option>
            </select>
          </div>
          <div>
            <label class="block mb-2 text-sm font-medium text-gray-900">How many midranges?</label>
            <select v-model="midrangeCount" class="bg-gray-50 border border-gray-300 text-gray-900 text-sm rounded-lg focus:ring-blue-500 focus:border-blue-500 block w-full p-2.5 dark:bg-gray-700 dark:border-gray-600 dark:placeholder-gray-400 dark:text-white dark:focus:ring-blue-500 dark:focus:border-blue-500">
              <option value="0">0</option>
              <option value="1">1</option>
              <option value="2">2</option>
              <option value="3">3</option>
              <option value="4">4</option>
              <option value="5">5</option>
            </select>
          </div>
          <div>
            <label  class="block mb-2 text-sm font-medium text-gray-900">How many drivers?</label>
            <select v-model="driverCount" class="bg-gray-50 border border-gray-300 text-gray-900 text-sm rounded-lg focus:ring-blue-500 focus:border-blue-500 block w-full p-2.5 dark:bg-gray-700 dark:border-gray-600 dark:placeholder-gray-400 dark:text-white dark:focus:ring-blue-500 dark:focus:border-blue-500">
              <option value="0">0</option>
              <option value="1">1</option>
              <option value="2">2</option>
              <option value="3">3</option>
              <option value="4">4</option>
              <option value="5">5</option>
            </select>
          </div>
        </div>
        <div class="roller pt-12">
          <button @click.prevent="generateBag" type="submit" class="focus:outline-none text-white bg-yellow-400 hover:bg-yellow-500 focus:ring-4 focus:ring-yellow-300 font-medium rounded-lg text-sm px-5 py-2.5 mr-2 mb-2 dark:focus:ring-yellow-900">
            Roll
          </button>
        </div>
      </div>
    </form>

    
    <div class="results pt-12">
        <div class="grid grid-cols-3 gap-4">
          <div>
            <label class="font-bold" for="">Putters</label>
            <div>
              <div v-for="disc in randomBag.putters" :key="disc.Name">
                {{ disc.Name }}
              </div>
            </div>
          </div>
          <div>
            <label class="font-bold" for="">Midranges</label>
            <div>
              <div v-for="disc in randomBag.midranges" :key="disc.Name">
                {{ disc.Name }}
              </div>
            </div>
          </div>
          <div>
            <label class="font-bold" for="">Drivers</label>
            <div>
              <div v-for="disc in randomBag.drivers" :key="disc.Name">
                {{ disc.Name }}
              </div>
            </div>
          </div>
      </div>
    </div>
  </div>
</template>

<style>
</style>