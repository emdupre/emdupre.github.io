<template>
  <b-container fluid id="pubs">
    <div v-if="loading" class="d-flex justify-content-center mb-3">
      <b-spinner variant="primary"></b-spinner>
    </div>
    <div v-else class="container-fluid">
      <h5>Journal Articles</h5>
      <b-list-group>
        <b-list-group-item v-for="(work, index) in works" :key="index">
          <PubItem
          :pub="work['work-summary'][0]"
          :doi="work['work-summary'][0]['external-ids']['external-id'][0]['external-id-value']">
          </PubItem>
        </b-list-group-item>
      </b-list-group>
      <br />
      <h5>Book Chapters</h5>
      <b-list-group>
        <b-list-group-item>
          <b-row align-h="between">
            <b-col cols="12">
              <div class="d-flex justify-content-between">
                <a class="mb-1" href="https://github.com/emdupre/emdupre.github.io/blob/bootstrap/assets/pubs/dupre_inpress_oxford.pdf" target="_blank">
                  Rumination Is a Sticky Form of Spontaneous Thought
                </a>
              </div>
                <span><span style="text-decoration:underline; color:black">E DuPre</span>, R Spreng.</span>
              <p class="mb-1">
                <small>The Oxford Handbook of Spontaneous Thought: Mind-Wandering, Creativity, and Dreaming (2018)</small>
              </p>
            </b-col>
          </b-row>
        </b-list-group-item>
      </b-list-group>
    </div>
  </b-container>
</template>

<script>
import Vue from 'vue';
import BootstrapVue from 'bootstrap-vue';
import PubItem from './PubItem'
Vue.use(BootstrapVue);

export default {
  name: 'Bloom',
  props: {},
  components: {
    PubItem,
  },
  data() {
    return {
      works: [],
      loading: true,
    };
  },
  mounted() {
    this.getOrcid('0000-0003-1358-196X');  // Replace with your own ORCID !
  },
  methods: {
    getOrcid(orcid) {
      const options = {
          method: 'GET',
          headers: new Headers({'accept': 'application/json'})
      };
      fetch(`https://pub.orcid.org/v3.0/${orcid}/works`, options)
      .then((resp) => resp.json())
      .then(data => {
        this.works = data.group;
        this.loading = false;
        // Check if a DOI is available:
        // console.log(this.works[0]['work-summary'][0]['external-ids']['external-id'][0]['external-id-value'])
        return this.works;
      });
    }
  },
};
</script>

<style>
</style>
