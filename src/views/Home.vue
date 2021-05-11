<template>
  <div class="home">
        <HeaderShayna />
        <HeroSection />  
        <BannerShayna />  
        <InstaShayna />  
        <PartnerShayna />
        <FooterShayna />
  </div>
</template>

<style scoped>
  .hero-section .hero-items .single-hero-items {
    background-image: url('/img/hero-1jpg');
  }
</style>

<script>
// @ is an alias to /src

import HeaderShayna from '@/components/HeaderShayna'
import HeroSection from '@/components/HeroSection'
import BannerShayna from '@/components/BannerShayna'
import InstaShayna from '@/components/InstaShayna'
import PartnerShayna from '@/components/PartnerShayna'
import FooterShayna from '@/components/FooterShayna'

export default {
  name: 'Home',
  components: {
    HeaderShayna,
    HeroSection,
    BannerShayna,
    InstaShayna,
    PartnerShayna,
    FooterShayna
  }
}
</script>
