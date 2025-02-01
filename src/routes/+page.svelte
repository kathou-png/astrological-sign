<script lang="ts">
	import { onMount } from 'svelte';
	let astrologicalValue: string = '';
	const astrologicalSigns = [
		{ name: 'aries', start: '03-21', end: '04-19' },
		{ name: 'taurus', start: '04-20', end: '05-20' },
		{ name: 'gemini', start: '05-21', end: '06-20' },
		{ name: 'cancer', start: '06-21', end: '07-22' },
		{ name: 'leo', start: '07-23', end: '08-22' },
		{ name: 'virgo', start: '08-23', end: '09-22' },
		{ name: 'libra', start: '09-23', end: '10-22' },
		{ name: 'scorpio', start: '10-23', end: '11-21' },
		{ name: 'sagittarius', start: '11-22', end: '12-21' },
		{ name: 'capricorn', start: '12-22', end: '01-19' },
		{ name: 'aquarius', start: '01-20', end: '02-18' },
		{ name: 'pisces', start: '02-19', end: '03-20' }
	];

	const getAstrologicalSign = (date: string): string | undefined => {
		const dateObj = new Date(date);
		const month = (dateObj.getMonth() + 1).toString().padStart(2, '0'); // MM
		const day = dateObj.getDate().toString().padStart(2, '0'); // DD
		const formattedDate = `${month}-${day}`; // MM-DD

		return astrologicalSigns.find((sign) => {
			const startMonth = parseInt(sign.start.split('-')[0]);
			const endMonth = parseInt(sign.end.split('-')[0]);

			if (startMonth > endMonth) {
				// Pour les signes comme Capricorne qui traversent l'année
				return formattedDate >= sign.start || formattedDate <= sign.end;
			} else {
				return formattedDate >= sign.start && formattedDate <= sign.end;
			}
		})?.name;
	};

	onMount(() => {
		const resultParagraph = document.querySelector('p');
		const day: HTMLInputElement | null = document.querySelector('#day');
		const month: HTMLInputElement | null = document.querySelector('#month');
		const year: HTMLInputElement | null = document.querySelector('#year');
		const date = new Date(
			parseInt(year?.value || ''),
			parseInt(month?.value || ''),
			parseInt(day?.value || '')
		);

		const updateSign = () => {
			if (!date) {
				astrologicalValue = 'Enter a valid';
				return;
			}
			if (day && month && year && resultParagraph) {
				const sign = getAstrologicalSign(`${year.value}-${month.value}-${day.value}`);
				astrologicalValue = sign || '';
				resultParagraph.textContent = `Your astrological sign is ${sign || 'unknown'}`;
			}
		};
		if (day) {
			updateSign();
			day.addEventListener('input', updateSign);
		}
		if (month) {
			updateSign();
			month.addEventListener('input', updateSign);
		}
		if (year) {
			updateSign();
			year.addEventListener('input', updateSign);
		}
		if (resultParagraph && day && month && year) {
			resultParagraph.textContent = `Your astrological sign is ${getAstrologicalSign(`${year.value}-${month.value}-${day.value}`) || 'unknown'}`;
		}
	});
</script>

<section class="flex h-screen w-screen flex-col bg-red-400">
	<div
		class="flex-between m-8 flex h-full flex-col items-center justify-center gap-10 rounded-xl bg-orange-50"
	>
		<h1 class="text-center text-3xl font-bold text-red-400">Astrological Sign</h1>

		<div class="grid grid-cols-3 gap-4">
			<div>
				<label for="day" class="mb-1 block">Day</label>
				<select id="day" class="w-full rounded border border-gray-300 p-2">
					<script>
						for (let i = 1; i <= 31; i++) {
							document.write(`<option value="${i}">${i}</option>`);
						}
					</script>
				</select>
			</div>

			<div>
				<label for="month" class="mb-1 block">Month</label>
				<select id="month" class="w-full rounded border border-gray-300 p-2">
					<option value="1">January</option>
					<option value="2">February</option>
					<option value="3">March</option>
					<option value="4">April</option>
					<option value="5">May</option>
					<option value="6">June</option>
					<option value="7">July</option>
					<option value="8">August</option>
					<option value="9">September</option>
					<option value="10">October</option>
					<option value="11">November</option>
					<option value="12">December</option>
				</select>
			</div>

			<div>
				<label for="year" class="mb-1 block">Year</label>
				<select id="year" class="w-full rounded border border-gray-300 p-2">
					<script>
						const currentYear = new Date().getFullYear();
						for (let i = currentYear; i >= currentYear - 100; i--) {
							document.write(`<option value="${i}">${i}</option>`);
						}
					</script>
				</select>
			</div>
		</div>

		<p class="w-full text-center"></p>
	</div>
</section>

<style>
	* {
		font-family: 'Modak', serif;
		margin: 0;
		font-size: 2rem;
		color: #fca5a1;
	}
	h1 {
		height: 'fit-content';
		font-family: 'Modak', serif;
		font-weight: normal;
		font-size: 6rem;
		height: 6rem;
	}
</style>
