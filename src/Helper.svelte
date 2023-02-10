<script>
  export let mod;

  function unconvertedEntities(mod) {
    let entities = [];
    for (const key in mod.entityList) {
      if (
        Object.hasOwnProperty.call(mod.entityList, key) &&
        mod.entityList[key].includes("ahorn") &&
        !mod.entityList[key].includes("loenn") &&
        !mod.entityList[key].includes("mlp")
      ) {
        entities.push(key);
      }
    }
    entities.sort();
    return entities;
  }

  function unconvertedTriggers(mod) {
    let triggers = [];
    for (const key in mod.triggerList) {
      if (
        Object.hasOwnProperty.call(mod.triggerList, key) &&
        mod.triggerList[key].includes("ahorn") &&
        !mod.triggerList[key].includes("loenn") &&
        !mod.triggerList[key].includes("mlp")
      ) {
        triggers.push(key);
      }
    }
    triggers.sort();
    return triggers;
  }

  function unconvertedEffects(mod) {
    let effects = [];
    for (const key in mod.effectList) {
      if (
        Object.hasOwnProperty.call(mod.effectList, key) &&
        mod.effectList[key].includes("ahorn") &&
        !mod.effectList[key].includes("loenn") &&
        !mod.effectList[key].includes("mlp")
      ) {
        effects.push(key);
      }
    }
    effects.sort();
    return effects;
  }

  function numUnconvertedPlugins(mod) {
    return (
      unconvertedEntities(mod).length +
      unconvertedTriggers(mod).length +
      unconvertedEffects(mod).length
    );
  }
</script>

<article>
  <h3>
    {mod.modName} <a href="https://gamebanana.com/mods/{mod.itemid}">🍌</a> ({mod.dependentCount} dependents)
  </h3>
  <details>
    {#if numUnconvertedPlugins(mod) == 1}
      <summary>1 plugin left</summary>
    {:else}
      <summary>{numUnconvertedPlugins(mod)} plugins left</summary>
    {/if}

    {#if unconvertedEntities(mod).length > 0}
      <h4>Entities</h4>
      <ul>
        {#each unconvertedEntities(mod) as entity}
          <li>{entity}</li>
        {/each}
      </ul>
    {/if}

    {#if unconvertedTriggers(mod).length > 0}
      <h4>Triggers</h4>
      <ul>
        {#each unconvertedTriggers(mod) as trigger}
          <li>{trigger}</li>
        {/each}
      </ul>
    {/if}

    {#if unconvertedEffects(mod).length > 0}
      <h4>Effects</h4>
      <ul>
        {#each unconvertedEffects(mod) as effect}
          <li>{effect}</li>
        {/each}
      </ul>
    {/if}
  </details>
</article>
