<script lang="ts">
  import { supportCards } from '../support-cards';

  interface SupportCard {
    name: string;
    title: string;
    rarity: 'SR' | 'SSR';
    type: 'スピード' | 'スタミナ' | 'パワー' | '根性' | '賢さ' | '友人';
    skills: string[];
    events: string[];
  }

  export let selectedSkills: string[] = [];

  function toId(card: SupportCard) {
    return card.name + '$' + card.title;
  }

  const cardById = new Map<string, SupportCard>();
  const cardIdsBySkill = new Map<string, Set<string>>();
  for (const card of supportCards) {
    const cardId = toId(card);
    cardById.set(cardId, card);
    for (const skill of [...card.skills, ...card.events]) {
      if (!cardIdsBySkill.has(skill)) {
        cardIdsBySkill.set(skill, new Set<string>());
      }
      cardIdsBySkill.get(skill).add(cardId);
    }
  }

  let combinedCardsList: SupportCard[][] = [];

  $: {
    if (selectedSkills.length >= 1) {
      const cardIds = cardIdsBySkill.get(selectedSkills[0]);
      if (cardIds != null) {
        combinedCardsList = [...cardIds].map((cardId) => [
          cardById.get(cardId),
        ]);
      }
    } else {
      combinedCardsList = [];
    }
  }
</script>

<div class="container">
  <ul>
    {#each combinedCardsList as combinedCards}
      <li>
        {#each combinedCards as combinedCard}
          <span>【{combinedCard.title}】{combinedCard.name}</span>
        {/each}
      </li>
    {/each}
  </ul>
</div>

<style lang="scss">
  .container {
    line-height: 200%;
  }
</style>
