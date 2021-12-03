<template>
  <div class="wrapper">
    <div v-if="!cardOpened">
      <person v-for="man in persons" :key="man.id" :name="man.name" :id="man.id" @personClick="openCard">
        <img src="../i/default-img.png" alt="">
      </person>
    </div>
    <div v-else>
      <div class="activePerson">
        <img src="../i/back-icon.png" alt="" @click="closeCard">
        <img src="../i/default-img.png" alt="">
        <h2>{{selectedPerson.name}}</h2>
      </div>
      <div class="friends">
        <h2>Друзья</h2>
        <person v-for="friend in getFriends" :key="friend.id" :name="friend.name" :id="friend.id" @personClick="openCard">
          <img src="../i/man.png" alt="">
        </person>
      </div>
      <div class="notFriends">
        <h2>Не в друзьях</h2>
        <person v-for="man in getPotentialFriends()" :key="man.name" :name="man.name" :id="man.id" @personClick="openCard">
          <img src="../i/man.png" alt="">
        </person>
      </div>
      <div class="popularPersons">
        <h2>Популярные люди</h2>
        <person v-for="man in getPopularPersons" :key="man[2]" :name="man[0]" :id="man[2]" @personClick="openCard">
          <img src="../i/man.png" alt="">
        </person>
      </div>
    </div>
  </div>
</template>

<script>
import Person from "@/components/Person";

export default {
  name: "mainPage",
  components: {
    Person
  },
  props: {
    persons: {
      type: Array,
      default: []
    }
  },
  data() {
    return {
      currentId: 1,
      amount: 3,
      cardOpened: false,
      popularPersons: [],
    }
  },
  methods: {
    openCard(id) {
      this.currentId = id
      this.cardOpened = true
    },
    closeCard() {
      this.cardOpened = false
    }
  },
  created() {
    let popularIndexes = []
    let result = []
    let obj = {}

    // считаем вхождения в массивах друзей
    for (let man of this.persons) {
      man.friends.forEach(el => {
        obj[el] ? obj[el]++ : obj[el] = 1
      })
    }
    // сортируем по кол-ву вхождений и добавляем индекс к имени, чтобы не было конфликта при одинаковых именах
    let sorted = Object.entries(obj)
      .map(el => {
        const person = this.persons.find(item => item.id == el[0])
        el.push(el[0])
        el[0] = person.name
        return el
      })
      .sort((a,b) => ((b[1] - a[1])))
    // сортируем по имени при равных количествах вхождений
    let sortedObjectsCounters = new Set(Object.values(Object.fromEntries(sorted)))
    for (let i of sortedObjectsCounters) {
      let myArr = sorted.filter(el => el[1] == i)
      result.push(myArr)
    }
    result.forEach(arr => {
      arr.sort((a,b) => {
        if (a[0] < b[0]) return -1
        if (a[0] > b[0]) return 1
        return 0
      })
      popularIndexes.push(...arr)
    })
    // результат
    this.popularPersons = popularIndexes
  },
  computed: {
    selectedPerson() {
      return this.persons.find(el => el.id == this.currentId)
    },
    getPopularPersons() {
      return this.popularPersons.filter(el => el[2] != this.currentId).slice(0, this.amount)
    },
    getFriends() {
      return this.persons.filter(el => this.selectedPerson.friends.includes(el.id))
    },
    getPotentialFriends() {
      return function () {
        let potentialFriends = this.persons
            .filter(el => !this.selectedPerson.friends.includes(el.id) && el.id !== this.selectedPerson.id)
            .sort(() => Math.random() - Math.random())
        return potentialFriends.slice(0, this.amount)
      }
    }
  }
}
</script>

<style scoped>
  img {
    width: 80px;
    height: 80px;
    border-radius: 50px;
  }

  .activePerson {
    display: flex;
    flex-direction: row;
    justify-content: space-around;
    width: 400px;
    align-items: center;
    cursor: pointer;
    padding: 10px;
    border-bottom: 1px solid #2c3e50;
    background-image: url("../i/background-active.jpg");
  }

  .wrapper {
    max-width: 1400px;
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
  }

</style>