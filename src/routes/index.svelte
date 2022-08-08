<script>
	let hasWindow = typeof window !== 'undefined';
	const storedText =
		hasWindow && !!window.localStorage.getItem('physicalText')
			? window.localStorage.getItem('physicalText')
			: null;
	const storedWritingMode =
		hasWindow && !!window.localStorage.getItem('writingMode')
			? window.localStorage.getItem('writingMode')
			: null;

	const storedApplyStyles =
		hasWindow && !!window.localStorage.getItem('applyStyles')
			? window.localStorage.getItem('applyStyles') === 'true'
			: true;

	let physicalText = storedText || `border-right: 1rem solid purple;`;
	let writingMode = storedWritingMode || 'horizontal-tb';
	let applyStyles = storedApplyStyles;

	$: hasWindow && window.localStorage.setItem('physicalText', physicalText),
		hasWindow && window.localStorage.setItem('writingMode', writingMode),
		hasWindow && window.localStorage.setItem('applyStyles', applyStyles.toString());

	// most of these are from here -
	// https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_Logical_Properties/Margins_borders_padding
	$: logicalText = physicalText
		.replaceAll('margin-top', 'margin-block-start')
		.replaceAll('margin-left', 'margin-inline-start')
		.replaceAll('margin-right', 'margin-inline-end')
		.replaceAll('margin-bottom', 'margin-block-end')

		.replaceAll('padding-top', 'padding-block-start')
		.replaceAll('padding-left', 'padding-inline-start')
		.replaceAll('padding-right', 'padding-inline-end')
		.replaceAll('padding-bottom', 'padding-block-end')

		// have to do the radius ones before the regular border ones
		.replaceAll('border-top-left-radius', 'border-start-start-radius')
		.replaceAll('border-bottom-left-radius', 'border-end-start-radius')
		.replaceAll('border-top-right-radius', 'border-start-end-radius')
		.replaceAll('border-bottom-right-radius', 'border-end-end-radius')

		.replaceAll('border-top', 'border-block-start')
		.replaceAll('border-left', 'border-inline-start')
		.replaceAll('border-right', 'border-inline-end')
		.replaceAll('border-bottom', 'border-block-end')

		.replaceAll('min-height', 'min-block-size')
		.replaceAll('max-height', 'max-block-size')
		.replaceAll('min-width', 'min-inline-size')
		.replaceAll('max-width', 'max-inline-size')
		
		.replaceAll('overflow-x', 'overflow-inline')
		.replaceAll('overflow-y', 'overflow-block')

		.replaceAll('text-align: left', 'text-align: start')
		.replaceAll('text-align: right', 'text-align: end')

		.replaceAll('float: left', 'float: start')
		.replaceAll('float: right', 'float: end')
		.replaceAll('clear: left', 'clear: start')
		.replaceAll('clear: right', 'clear: end')

		// important to do these very generic ones last
		.replaceAll('top', 'inset-block-start')
		.replaceAll('left', 'inset-inline-start')
		.replaceAll('right', 'inset-inline-end')
		.replaceAll('bottom', 'inset-block-end');

	// https://gist.github.com/nyurik/d438cb56a9059a0660ce4176ef94576f
</script>

<header>
	<h1>what are logical properties?</h1>

	<blockquote>
		<p>
			<strong>CSS Logical Properties and Values</strong> is a module of
			<a href="https://developer.mozilla.org/en-US/docs/Web/CSS">CSS</a> introducing logical properties
			and values that provide the ability to control layout through logical, rather than physical, direction
			and dimension mappings.
		</p>
		<p>
			Logical properties and values use the abstract terms <em>block</em> and <em>inline</em> to
			describe the direction in which they flow. The physical meaning of these terms depends on the
			<a href="https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_Writing_Modes">writing mode</a>.
		</p>

		<cite
			><a href="https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_Logical_Properties">⸺ MDN</a
			></cite
		>
	</blockquote>
</header>

<hr />

<main>
	{#if hasWindow && !!window.localStorage}
		<fieldset>
			<section>
				<label for="apply-styles">
					<input
						type="checkbox"
						id="apply-styles"
						bind:checked={applyStyles}
						autocomplete="false"
					/>
					<strong>apply styles</strong>
				</label>

				<label for="writing-mode">
					<strong>writing mode:</strong>
					<select name="writing-mode" id="writing-mode" bind:value={writingMode}>
						<option value="horizontal-tb">horizontal-tb</option>
						<option value="vertical-rl">vertical-rl</option>
						<option value="vertical-lr">vertical-lr</option>
						<option value="sideways-rl" title="experimental">sideways-rl 🧪</option>
						<option value="sideways-lr" title="experimental">sideways-lr 🧪</option>
					</select>
				</label>
			</section>

			<section>
				<label for="physical">
					<strong>physical properties:</strong>
					<textarea
						id="physical"
						style={applyStyles ? physicalText : ''}
						bind:value={physicalText}
					/>
				</label>

				<label for="logical">
					<strong>logical properties:</strong>
					<textarea
						id="logical"
						style={applyStyles
							? `writing-mode: ${writingMode}; ${logicalText}`
							: `writing-mode: ${writingMode}`}
						bind:value={logicalText}
					/>
				</label>
			</section>
		</fieldset>
	{/if}
</main>

<footer>
	<hr />
	<a href="https://github.com/ryanfiller/logical-property-converter">
		<strong>code</strong>
	</a>
	|
	<a href="https://github.com/ryanfiller/logical-property-converter/issues">
		<strong>bugs?</strong>
	</a>
	|
	<a href="https://www.ryanfiller.com?link=logical-properties">
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
		position: relative; /* this is to catch any `position: absolute` children */
	}

	label[for='apply-styles'] {
		display: flex;
	}

	label:not([for='writing-mode'], [for='apply-styles']) {
		flex: 1;
		flex-basis: calc(0.5 * var(--readable));
		display: flex;
		gap: 0.5rem;
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
