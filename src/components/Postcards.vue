<template>
    <div class="postcards">
      <table>
        <thead>
          <tr>
            <th id="th-first"></th>
            <th v-for="(header, hIndex) in postcardsHeader" :key="hIndex">{{ header }}</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="(postcard, pIndex) in postcardsList" :key="pIndex" :class="{'done': isDone(pIndex)}">
            <td v-for="index in (postcardsHeader.length + 1)"  :key="index">
                <p v-if="index === 1">{{ postcard }}</p>
                <label v-else class="custom-checkbox">
                    <input 
                        type="checkbox" 
                        :checked="checkboxStates[pIndex][index - 2]"
                        @change="handleCheckboxChange(pIndex, index - 2, $event)"
                    >
                    <span class="checkmark"></span>
                </label>
            </td>
          </tr>
        </tbody>
      </table>
    </div>
  </template>
  
  <script lang="ts">
import { defineComponent, reactive } from 'vue';

export default defineComponent({
  name: 'Postcards',
  props: {
    postcardsHeader: {
      type: Array,
      required: true
    },
    postcardsList: {
      type: Array,
      required: true
    }
  },
  setup(props) {
    const checkboxStates = reactive(
      props.postcardsList.map(() => Array(props.postcardsHeader.length).fill(false))
    );

    const handleCheckboxChange = (pIndex: number, hIndex: number, e: Event) => {
      const isChecked = (e.target as HTMLInputElement).checked;

      if (isChecked) {
        for (let i = 0; i <= hIndex; i++) {
          checkboxStates[pIndex][i] = true;
        }
      } else {
        for (let i = hIndex; i < checkboxStates[0].length; i++) {
          checkboxStates[pIndex][i] = false;
        }
      }
    };

    return {
      checkboxStates,
      handleCheckboxChange
    };
  },
  methods: {
    isDone: function(pIndex: number) {
        return !this.checkboxStates[pIndex].some((state) => state == false);
    }
  }
});
</script>

  
<style scoped>
.postcards {
    width: fit-content;
}

table {
    border-collapse: collapse;
}

th, td {
    border: 2px solid #fff;
    padding: 8px;
    min-width: 150px;
}

th {
    background-image: linear-gradient(#E0E0E0, #fff);
}

td {
    color: #5598D9;
    background-color: #F2F6FF;
    border-left-color: #F2F6FF;
    border-right-color: #F2F6FF;
    font-weight: bold;
}

tr.done td {
    color: #46A530;
    background-color: #BBFFA3;
}

.custom-checkbox {
    position: relative;
    display: inline-block;
    width: 24px;
    height: 24px;
}

.custom-checkbox input {
    opacity: 0;
    width: 0;
    height: 0;
}

.checkmark {
    position: absolute;
    top: 0;
    left: 0;
    height: 24px;
    width: 24px;
    background-image: linear-gradient(#FDFDFD, #E7E7E7);
    border-radius: 5px;
    border: 2px solid #CBCBCB;
    box-sizing: border-box;
}

.custom-checkbox input:checked ~ .checkmark {
    background-image: linear-gradient(#62A92C, #229F34);
    border: none;
}

.checkmark:after {
    content: "";
    position: absolute;
    display: none;
}

.custom-checkbox input:checked ~ .checkmark:after {
    display: block;
}

.custom-checkbox .checkmark:after {
    left: 8px;
    top: 4px;
    width: 6px;
    height: 12px;
    border: solid white;
    border-width: 0 2px 2px 0;
    transform: rotate(45deg);
}
</style>
