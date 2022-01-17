<script>
	import { Swiper, SwiperSlide } from 'swiper/svelte';
	// Import Swiper styles
	import 'swiper/css';

	import 'swiper/css/effect-coverflow';
	import 'swiper/css/pagination';

	import './style.css';

	// import Swiper core and required modules
	import SwiperCore, { EffectCoverflow, Pagination } from 'swiper';

	// install Swiper modules
	SwiperCore.use([EffectCoverflow, Pagination]);
	let data = [];
	const url = 'https://backend-node-alan13377.vercel.app/api/galeria';
	const getImages = async () => {
		const response = await fetch(url);
		data = await response.json();
	};
	getImages();
</script>

<Swiper
	effect={'coverflow'}
	grabCursor={true}
	centeredSlides={true}
	slidesPerView={'auto'}
	coverflowEffect={{
		rotate: 50,
		stretch: 0,
		depth: 100,
		modifier: 1,
		slideShadows: true
	}}
	pagination={true}
	class="mySwiper"
>
	{#each data as datos}
		<!-- content here -->
		<SwiperSlide><img loading="lazy" src={datos.image} /></SwiperSlide>
	{/each}
</Swiper>

<style>
	img {
		width: 100%;
		height: 100%;
	}
</style>
