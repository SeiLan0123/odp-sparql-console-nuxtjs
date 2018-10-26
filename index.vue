<template>
  <section class="container">
    <div class="query">
    <button @click="call" class="button--grey">検索</button>
      <p><textarea rows="40" cols="70" v-model="querySentence"></textarea></p>
    </div>
    <div class="result">
    {{data}}
    </div>
  </section>
</template>

<script>
import AppLogo from "~/components/AppLogo.vue";
import axios from "axios";
var querySentence;
var query;
var data;
export default {
  components: {},

  async asyncData({}) {
    data = "result";
    querySentence = `prefix bibo:            <http://purl.org/ontology/bibo/>
prefix cal:             <http://www.w3.org/2002/12/cal/icaltzd#>
prefix cc:              <http://creativecommons.org/ns#>
prefix dc:              <http://purl.org/dc/elements/1.1/>
prefix dcat:            <http://www.w3.org/ns/dcat#>
prefix dcterms:         <http://purl.org/dc/terms/>
prefix foaf:            <http://xmlns.com/foaf/0.1/>
prefix geo:             <http://www.w3.org/2003/01/geo/wgs84_pos#>
prefix georss:          <http://www.georss.org/georss>
prefix gr:              <http://purl.org/goodrelations/v1#>
prefix gtfs:            <http://vocab.gtfs.org/terms#>
prefix ic231:           <http://imi.ipa.go.jp/ns/core/rdf#>
prefix ic:              <http://imi.go.jp/ns/core/rdf#>
prefix ipa:             <http://imi.ipa.go.jp/ns/core/rdf#>
prefix jrrk:            <http://purl.org/jrrk#>
prefix odp:             <http://odp.jig.jp/odp/1.0#>
prefix owl:             <http://www.w3.org/2002/07/owl#>
prefix pc:              <http://purl.org/procurement/public-contracts#>
prefix qb:              <http://purl.org/linked-data/cube#>
prefix rdf:             <http://www.w3.org/1999/02/22-rdf-syntax-ns#>
prefix rdfs:            <http://www.w3.org/2000/01/rdf-schema#>
prefix sac:             <http://statdb.nstac.go.jp/lod/sac/C>
prefix sacs:            <http://statdb.nstac.go.jp/lod/terms/sacs#>
prefix schema:          <http://schema.org/>
prefix sdmx-concept:    <http://purl.org/linked-data/sdmx/2009/concept#>
prefix sdmx-dimension:  <http://purl.org/linked-data/sdmx/2009/dimension#>
prefix sdmx-measure:    <http://purl.org/linked-data/sdmx/2009/measure#>
prefix skos:            <http://www.w3.org/2004/02/skos/core#>
prefix xsd:             <http://www.w3.org/2001/XMLSchema#>

select * {?s ?p ?o.
    } limit 10`;
    console.log("async");
    return { data, querySentence };
  },

  methods: {
    async call() {
      if (!this.querySentence) {
        alert("querySentence is undifined");
      }
      console.log(this.querySentence);
      query =
        `https://sparql.odp.jig.jp/api/v1/sparql?output=json&query=` +
        encodeURIComponent(this.querySentence);
      console.log(query);
      var { data } = await axios.get(query);
      this.data = JSON.stringify(data, null, 4);
      console.log(this.data);
    }
  }
};
</script>

<style>
.container {
  min-height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
  text-align: center;
}

.title {
  font-family: "Quicksand", "Source Sans Pro", -apple-system, BlinkMacSystemFont,
    "Segoe UI", Roboto, "Helvetica Neue", Arial, sans-serif; /* 1 */
  display: block;
  font-weight: 300;
  font-size: 100px;
  color: #35495e;
  letter-spacing: 1px;
}

.subtitle {
  font-weight: 300;
  font-size: 42px;
  color: #526488;
  word-spacing: 5px;
  padding-bottom: 15px;
}

.links {
  padding-top: 15px;
}

.query {
  overflow-x: scroll;
  position: absolute;
  top: 10px;
  left: 1vw;
  width: 48vw;
  height: 100vh;
}

.result {
  overflow: scroll;
  position: absolute;
  font-size: 10pt;
  left: 51vw;
  top: 1vh;
  white-space: pre-wrap;
  text-align: left;
  width: 48vw;
  height: 100vh;
}
</style>
