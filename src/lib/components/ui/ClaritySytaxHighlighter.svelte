<script lang="ts">
	import prism from 'prismjs';

	export let sourceCode = '';
	let language = 'clarity';
	const par = '(\\()';
	// End the pattern with look-ahead space
	const space = '(?=\\s)';
	const lookup = () => {
		return RegExp('([\\s([])' + '[-]?u?\\d+' + '(?=[\\s)])');
	};

	prism.languages.clarity = {
		// Three or four semicolons are considered a heading.
		heading: {
			pattern: /;;;.*/,
			alias: ['comment', 'title']
		},
		comment: /;;.*/,
		string: [
			{
				pattern: /"(?:[^"\\]|\\.)*"/,
				greedy: true
			},
			{
				pattern: /0x[0-9a-fA-F]*/,
				greedy: true
			}
		],
		symbol: {
			pattern: /'[^()#'\s]+/,
			greedy: true
		},
		keyword: [
			{
				pattern: RegExp(
					par +
						'(?:or|and|xor|not|begin|let|if|ok|err|unwrap\\!|unwrap-err\\!|unwrap-panic|unwrap-err-panic|match|try\\!|asserts\\!|\
map-get\\?|var-get|contract-map-get\\?|get|tuple|\
define-public|define-private|define-constant|define-map|define-data-var|\
define-fungible-token|define-non-fungible-token|\
define-read-only)' +
						space
				),
				lookbehind: true
			},
			{
				pattern: RegExp(par + '(?:is-eq|is-some|is-none|is-ok|is-er)' + space),
				lookbehind: true
			},
			{
				pattern: RegExp(
					par +
						'(?:var-set|map-set|map-delete|map-insert|\
ft-transfer\\?|nft-transfer\\?|nft-mint\\?|ft-mint\\?|nft-get-owner\\?|ft-get-balance\\?|\
contract-call\\?)' +
						space
				),
				lookbehind: true
			},
			{
				pattern: RegExp(
					par +
						'(?:list|map|filter|fold|len|concat|append|as-max-len\\?|to-int|to-uint|\
buff|hash160|sha256|sha512|sha512/256|keccak256|true|false|none)' +
						space
				),
				lookbehind: true
			},
			{
				pattern: RegExp(
					par +
						'(?:as-contract|contract-caller|tx-sender|block-height|at-block|get-block-info\\?)' +
						space
				),
				lookbehind: true
			},
			{
				pattern: RegExp(par + '(?:is-eq|is-some|is-none|is-ok|is-err)' + space),
				lookbehind: true
			}
		],
		boolean: /(?:false|true|none)/,
		number: {
			pattern: lookup(),
			lookbehind: true
		},
		address: {
			pattern: /([\s()])(?:\'[0123456789ABCDEFGHJKMNPQRSTVWXYZ]{28,41})(?=[()\s]|$)/,
			lookbehind: true
		},
		operator: {
			pattern: /(\()(?:[-+*\/]|[<>]=?|=>?)(?=[()\s]|$)/,
			lookbehind: true
		},
		function: {
			pattern: /(\()[^()'\s]+(?=[()\s]|$)/,
			lookbehind: true
		},
		punctuation: /[()']/
	};
	// const prettySource = prism.highlight(sourceCode, prism.languages[language], 'clarity');
</script>

<div class="code m-5 rounded-lg border border-gray-500 p-5">
	{@html prism.highlight(sourceCode, prism.languages[language], 'clarity')}
</div>

<style>
	.code {
		white-space: pre-wrap;
		font-family: monospace;
	}
</style>
