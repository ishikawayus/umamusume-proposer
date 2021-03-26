<script lang="ts">
  import SkillSelector from '$lib/components/SkillSelector.svelte';
  import CardCombiner from '$lib/components/CardCombiner.svelte';

  let selectedSkills: string[] = [];

  function handleSelectedSkillsChange(event: CustomEvent) {
    selectedSkills = event.detail;
  }
</script>

<main>
  <SkillSelector on:selectedSkillsChange={handleSelectedSkillsChange} />
  <CardCombiner {selectedSkills} />
</main>

<style lang="scss">
  :root {
    font-family: 'Helvetica Neue', Arial, 'Hiragino Kaku Gothic ProN',
      'Hiragino Sans', Meiryo, sans-serif;
  }

  main {
    width: 800px;
    margin: 0 auto;
  }
</style>
