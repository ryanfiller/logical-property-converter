<script>
  let hasWindow = typeof window !== 'undefined'
  const storedText = (hasWindow && !!window.localStorage.getItem('physicalText'))
    ? window.localStorage.getItem('physicalText')
    : null
  const storedWritingMode = (hasWindow && !!window.localStorage.getItem('writingMode'))
    ? window.localStorage.getItem('writingMode')
    : null

  const storedApplyStyles = (hasWindow && !!window.localStorage.getItem('applyStyles'))
    ? window.localStorage.getItem('applyStyles') === 'true'
    : true

  let physicalText = storedText || `padding-left: 2rem;`
  let writingMode = storedWritingMode || 'horizontal-tb'
  let applyStyles = storedApplyStyles

  $: (
    hasWindow && window.localStorage.setItem('physicalText', physicalText),
    hasWindow && window.localStorage.setItem('writingMode', writingMode),
    hasWindow && window.localStorage.setItem('applyStyles', applyStyles.toString())
  )

  $: logicalText = physicalText
    .replace('left', 'inline-start')
    .replace('right', 'inline-end')

  // https://gist.github.com/nyurik/d438cb56a9059a0660ce4176ef94576f
</script>

<header>
  <h1>what are logical properties?</h1>

  <blockquote>
    <p>
      <strong>CSS Logical Properties and Values</strong> is a module of <a href='https://developer.mozilla.org/en-US/docs/Web/CSS'>CSS</a> introducing logical properties and values that provide the ability to control layout through logical, rather than physical, direction and dimension mappings.
    </p>
    <p>
      Logical properties and values use the abstract terms <em>block</em> and <em>inline</em> to describe the direction in which they flow. The physical meaning of these terms depends on the <a href='https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_Writing_Modes'>writing mode</a>.
    </p>

    <cite><a href='https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_Logical_Properties'>⸺ MDN</a></cite>
  </blockquote>
</header>

<hr />

<main>
  <fieldset>
    <section>
      <label for='apply-styles'>
        <input
          type='checkbox'
          id='apply-styles'
          bind:checked={applyStyles}
          autocomplete='false'
        >
        <strong>apply styles</strong>
      </label>

      <label for='writing-mode'>
        <strong>writing mode:</strong>
        <select
          name='writing-mode'
          id='writing-mode'
          bind:value={writingMode}
        >
          <option value='horizontal-tb'>horizontal-tb</option>
          <option value='vertical-rl'>vertical-rl</option>
          <option value='vertical-lr'>vertical-lr</option>
          <option value='sideways-rl'>sideways-rl 🧪</option>
          <option value='sideways-lr'>sideways-lr 🧪</option>
        </select>
      </label>
    </section>

    <section>
      <label for='physical'>
        <strong>physical properties:</strong>
        <textarea
          id='physical'
          style={applyStyles ? physicalText : ''}
          bind:value={physicalText}
        />
      </label>
  
      <label for='logical'>
        <strong>logical properties:</strong>
        <textarea
          id='logical'
          style={applyStyles ? `writing-mode: ${writingMode}; ${logicalText}` : `writing-mode: ${writingMode}`}
          bind:value={logicalText}
        />
      </label>
    </section>
  </fieldset>
</main>

<footer>
  <hr />
  <a href='https://www.ryanfiller.com/'>
    <strong>ryanfiller.com</strong>
  </a>
</footer>

<style>
  :root {
    --readable: 50rem;
  }

  :global(html),
  :global(body) {
    display: flex;
    flex-direction: column;
    gap: 1rem;
    padding: 0.5rem;
    height: calc(100vh - 1rem);
  }

  header {
    display: grid;
    gap: 0 1rem;
    grid-template-columns: auto minmax(auto, var(--readable)) auto;
  }

  header > * {
    grid-column: 2 / 3;
  }

  hr {
    width: 100%;
  }

  strong {
    display: inline;
  }

  main {
    flex: 1;
    display: flex;
    flex-direction: column;
  }
  
  fieldset {
    flex: 1;
    display: flex;
    flex-direction: column;
    gap: 1rem;
  }

  fieldset section {
    display: flex;
    gap: 1rem;
    flex-wrap: wrap;
  }

  fieldset section:nth-child(1) {
    display: flex;
    gap: 1rem;
    justify-content: space-between;
  }

  fieldset section:nth-child(2) {
    flex: 1;
    display: flex;
    gap: 1rem;
  }

  label[for='apply-styles'] {
    display: flex;
  }
  
  label:not([for='writing-mode'], [for='apply-styles']) {
    flex: 1;
    flex-basis: calc(0.5 *  var(--readable));
    display: flex;
    gap: .5rem;
    flex-direction: column;
  }

  textarea {
    flex: 1;
    resize: none;
    min-height: 25vh;
    box-sizing: border-box;
  }

  footer {
    display: flex;
    gap: 1rem;
    align-items: center;
  }
</style>