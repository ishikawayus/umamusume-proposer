<script lang="ts">
  import { createEventDispatcher } from 'svelte';
  import { supportCards } from '../support-cards';

  const dispatch = createEventDispatcher();

  const skillSet = new Set<string>();
  for (const supportCard of supportCards) {
    supportCard.skills.forEach((skill) => skillSet.add(skill));
    supportCard.events.forEach((skill) => skillSet.add(skill));
  }
  const skills = [...skillSet].sort();

  const selectedSkills = new Set<string>();

  function handleCheckboxChange(event: Event) {
    if (!(event.target instanceof HTMLInputElement)) {
      return;
    }
    if (event.target.checked) {
      selectedSkills.add(event.target.value);
    } else {
      selectedSkills.delete(event.target.value);
    }
    dispatch('selectedSkillsChange', [...selectedSkills].sort());
  }
</script>

<div class="container">
  {#each skills as skill, index}
    <span class="skill">
      <input
        type="checkbox"
        id="skill-{index}"
        name="skill-{index}"
        class="skill-checkbox"
        value={skill}
        on:change={handleCheckboxChange}
      />
      <label for="skill-{index}" class="skill-label">{skill}</label>
    </span>
  {/each}
</div>

<style lang="scss">
  .container {
    line-height: 200%;
  }
  .skill-checkbox {
    display: none;
  }
  .skill-label {
    padding: 4px;
    border: solid 1px #999999;
    border-radius: 4px;
    white-space: nowrap;
    cursor: pointer;
  }
  .skill-checkbox:checked + .skill-label {
    background: #ffff99;
  }
</style>
