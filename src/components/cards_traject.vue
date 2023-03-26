<template>
  <div>
    <div v-for="(card, index) in cards" :key="index" class="card">
      <div class="card-header">
        <h3>{{ card.name }}</h3>
        <button @click="deleteCard(index)">Supprimer</button>
      </div>
      <div class="card-body">
        <div
          v-for="(dropdown, dropdownIndex) in card.dropdowns"
          :key="dropdownIndex"
          class="dropdown"
        >
          <select v-model="dropdown.value">
            <option
              v-for="(option, optionIndex) in dropdown.options"
              :key="optionIndex"
            >
              {{ option }}
            </option>
          </select>
          <button
            @click="deleteDropdown(card, dropdownIndex)"
            class="cross-button"
          >
            <svg viewBox="0 0 10 10" width="16" height="16">
              <line x1="2" y1="2" x2="8" y2="8" stroke-width="2" />
              <line x1="2" y1="8" x2="8" y2="2" stroke-width="2" />
            </svg>
          </button>
        </div>
        <div v-if="card.dropdowns.length < 4" :key="index" class="addbutton">
          <button @click="addDropdown(card)">
            Ajouter une liste déroulante
          </button>
        </div>
      </div>
    </div>
    <button @click="addCard()">Ajouter une carte</button>
  </div>
</template>

<script lang="ts">
import { ref } from 'vue';

interface Dropdown {
  value: string;
  options: string[];
}

interface Card {
  name: string;
  dropdowns: Dropdown[];
}

interface showAddButton {}

export default {
  name: 'CardList',
  data() {
    return {
      cards: [
        {
          name: 'Carte 1',
          dropdowns: [
            {
              value: 'Option 1.1',
              options: ['Option 1.1', 'Option 1.2', 'Option 1.3'],
            },
            {
              value: 'Option 2.1',
              options: ['Option 2.1', 'Option 2.2', 'Option 2.3'],
            },
          ],
        },
      ],
    };
  },
  methods: {
    addCard() {
      const card: Card = {
        name: `Carte ${this.cards.length + 1}`,
        dropdowns: [],
      };
      this.cards.push(card);
      this.addDropdown(card);
    },
    deleteCard(index: number) {
      this.cards.splice(index, 1);
    },
    // addDropdown(card: Card) {
    //   card.dropdowns.push({ value: '', options: [] });
    // },
    addDropdown(card: Card) {
      if (card.dropdowns.length < 4) {
        const dropdown: Dropdown = {
          value: 'Option ' + `${card.dropdowns.length + 1}` + '.1',
          options: [],
        };
        for (let i = 1; i <= 3; i++) {
          dropdown.options.push(
            'Option ' + `${card.dropdowns.length + 1}.${i}`
          );
        }
        card.dropdowns.push(dropdown);
      }
      console.log(card.dropdowns.length);
    },
    deleteDropdown(card: Card, index: number) {
      card.dropdowns.splice(index, 1);
    },
  },
};
</script>

<style scoped>
.card {
  margin: 20px;
  padding: 10px;
  border: 1px solid black;
  border-radius: 5px;
}

.card-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.card-body {
  margin-top: 10px;
}

.dropdown {
  margin-bottom: 10px;
}

.cross-button svg {
  stroke: currentColor;
  stroke-linecap: round;
  transition: stroke 0.2s ease-in-out;
}
.cross-button:hover {
  border-color: red;
}
.cross-button:hover svg {
  stroke-width: 3;
  border-color: red;
  stroke: red;
  transition: opacity 0.2s ease-in-out;
}
.cross-button:active svg {
  stroke-width: 2;
  stroke: red;
}
</style>
