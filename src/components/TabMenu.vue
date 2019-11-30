<template>
  <div class="tabMenu">
    <button
      v-for="tab in tabs"
      :key="tab"
      :class="['tab-btn', { active: selected === tab }]"
      @click="selected = tab;"
    >
      {{ tab }}
    </button>

    <component :is="selected" class="tab" />
  </div>
</template>

<script>
import Form from "./Form";
import CodeArea from './Code'

export default {
	components: {
    Form,
    CodeArea
	},
	data: function() {
		return {
			tabs: ["Form","CodeArea"],
			selected: "Form"
		};
	}
};
</script>

<style>
  .tabMenu{
    /* padding: 10px; */
    /* background: #FFDC80; */
    width: 100%;
  }

  .tab-btn {
    padding: 6px 10px;
    background: #ffffff;
    cursor: pointer;
    /* margin-bottom: 1rem; */
    /* border: 2px solid #cccccc; */
    outline: none;
  }

  .active {
    border-bottom: 3px solid green;
    background: #ffff25;
  }

  .tab {
    /* border: 1px solid #ccc; */
    padding: 10px;
  }
</style>