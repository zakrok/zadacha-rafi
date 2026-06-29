<script>
  let bill = $state(85.5);
  let people = $state(1);

  const presets = [
    { label: '10%', value: 10 },
    { label: '15%', value: 15 },
    { label: '20%', value: 2 },
  ];
  let selectedIndex = $state(1);

  function selectPreset(i) {
    selectedIndex = i;
  }

  function changePeople(delta) {
    const next = people + delta;
    if (next >= 1) people = next;
  }

  const rate = $derived(presets[selectedIndex].value);
  const tipAmount = $derived(bill * rate / 100);
  const total = $derived(bill);
  const perPerson = $derived(total / people);
</script>

<main>
  <section class="card" aria-label="Bill splitter">
    <header class="head">
      <h1>Split the bill</h1>
      <p class="sub">Work out the tip and what everyone owes.</p>
    </header>

    <div class="field">
      <label for="bill">Bill amount</label>
      <div class="money-input">
        <span class="prefix">$</span>
        <input
                id="bill"
                type="number"
                min="0"
                step="0.01"
                bind:value={bill}
                inputmode="decimal"
        />
      </div>
    </div>

    <div class="field">
      <span class="field-label">Tip</span>
      <div class="presets" role="group" aria-label="Tip percentage">
        {#each presets as p, i}
          <button
                  type="button"
                  class="preset"
                  class:active={selectedIndex === i}
                  onclick={() => selectPreset(i)}
          >
            {p.label}
          </button>
        {/each}
      </div>
    </div>

    <div class="field">
      <span class="field-label">People</span>
      <div class="stepper">
        <button type="button" class="step" onclick={() => changePeople(-1)} aria-label="Fewer people">–</button>
        <span class="count" aria-live="polite">{people}</span>
        <button type="button" class="step" onclick={() => changePeople(1)} aria-label="More people">+</button>
      </div>
    </div>

    <div class="summary">
      <div class="row">
        <span class="r-label">Tip</span>
        <span class="r-value">${tipAmount}</span>
      </div>
      <div class="row">
        <span class="r-label">Total</span>
        <span class="r-value">${total.toFixed(2)}</span>
      </div>
      <div class="row total">
        <span class="r-label">Each person pays</span>
        <span class="r-value accent">${perPerson.toFixed(2)}</span>
      </div>
    </div>
  </section>
</main>

<style>
  :global(body) {
    margin: 0;
    background: #f7f8fa;
    color: #1b1f2a;
    font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
  }

  main {
    min-height: 100vh;
    display: grid;
    place-items: center;
    padding: 24px;
    box-sizing: border-box;
  }

  .card {
    width: 100%;
    max-width: 380px;
    background: #ffffff;
    border: 1px solid #e5e7eb;
    border-radius: 16px;
    padding: 28px;
    box-shadow: 0 1px 2px rgba(16, 24, 40, 0.04), 0 8px 24px rgba(16, 24, 40, 0.06);
    box-sizing: border-box;
  }

  .head { margin-bottom: 22px; }
  h1 {
    font-size: 22px;
    font-weight: 650;
    letter-spacing: -0.02em;
    margin: 0;
  }
  .sub {
    margin: 6px 0 0;
    color: #6b7280;
    font-size: 14px;
  }

  .field { margin-bottom: 18px; }

  label, .field-label {
    display: block;
    font-size: 13px;
    font-weight: 600;
    color: #374151;
    margin-bottom: 8px;
  }

  .money-input {
    display: flex;
    align-items: center;
    border: 1px solid #e5e7eb;
    border-radius: 10px;
    padding: 0 12px;
    transition: border-color 0.15s, box-shadow 0.15s;
  }
  .money-input:focus-within {
    border-color: #0e9f6e;
    box-shadow: 0 0 0 3px rgba(14, 159, 110, 0.15);
  }
  .prefix {
    color: #9ca3af;
    font-family: ui-monospace, SFMono-Regular, Menlo, monospace;
    font-size: 16px;
  }
  input[type='number'] {
    border: 0;
    outline: 0;
    width: 100%;
    padding: 12px 8px;
    font-size: 16px;
    font-family: ui-monospace, SFMono-Regular, Menlo, monospace;
    background: transparent;
    color: inherit;
  }

  .presets {
    display: grid;
    grid-template-columns: repeat(3, 1fr);
    gap: 8px;
  }
  .preset {
    padding: 11px 0;
    border: 1px solid #e5e7eb;
    background: #fff;
    border-radius: 10px;
    font-size: 15px;
    font-weight: 600;
    color: #374151;
    cursor: pointer;
    transition: all 0.12s;
  }
  .preset:hover { border-color: #d1d5db; }
  .preset.active {
    background: #e7f6ef;
    border-color: #0e9f6e;
    color: #0a7a54;
  }
  .preset:focus-visible {
    outline: 2px solid #0e9f6e;
    outline-offset: 2px;
  }

  .stepper {
    display: inline-flex;
    align-items: center;
    gap: 4px;
    border: 1px solid #e5e7eb;
    border-radius: 10px;
    padding: 4px;
  }
  .step {
    width: 38px;
    height: 38px;
    border: 0;
    background: #f3f4f6;
    border-radius: 8px;
    font-size: 20px;
    line-height: 1;
    color: #374151;
    cursor: pointer;
    transition: background 0.12s;
  }
  .step:hover { background: #e5e7eb; }
  .step:focus-visible {
    outline: 2px solid #0e9f6e;
    outline-offset: 2px;
  }
  .count {
    min-width: 44px;
    text-align: center;
    font-family: ui-monospace, SFMono-Regular, Menlo, monospace;
    font-size: 17px;
    font-weight: 600;
  }

  .summary {
    margin-top: 24px;
    padding-top: 20px;
    border-top: 1px dashed #e5e7eb;
  }
  .row {
    display: flex;
    justify-content: space-between;
    align-items: baseline;
    padding: 8px 0;
  }
  .r-label { color: #6b7280; font-size: 14px; }
  .r-value {
    font-family: ui-monospace, SFMono-Regular, Menlo, monospace;
    font-size: 16px;
    font-variant-numeric: tabular-nums;
  }
  .row.total {
    margin-top: 6px;
    padding-top: 14px;
    border-top: 1px solid #f1f2f4;
  }
  .row.total .r-label {
    color: #1b1f2a;
    font-weight: 600;
    font-size: 15px;
  }
  .r-value.accent {
    color: #0e9f6e;
    font-size: 22px;
    font-weight: 650;
  }

  @media (max-width: 420px) {
    .card { padding: 22px; }
  }
</style>
