<template>
  <div>
    <h2 class="is-size-2">
      Server
    </h2>

    <div class="columns">
      <div class="column">
        <b-field label="Inference URL">
          <b-input
            v-model="inferenceURL"
            placeholder="inference server URL"
          />
        </b-field>
      </div>
      <div class="column">
        <b-field label="GraphQL URL">
          <b-input
            v-model="graphqlURL"
            placeholder="GraphQL endpoint URL"
          />
        </b-field>
      </div>
    </div>
    <hr>
    <h2 class="is-size-2">
      Generation
    </h2>
    <div class="columns">
      <div class="column is-one-quarter">
        <b-field label="Database to use">
          <b-select v-model="schemaId">
            <option
              disabled
              value=""
            >
              Please select one
            </option>
            <option
              v-for="id in schemaIds"
              :key="id"
            >
              {{ id }}
            </option>
          </b-select>
        </b-field>
      </div>
      <div class="column">
        <b-field label="English prompt">
          <b-input
            v-model="prompt"
            style="width: 100%;"
            placeholder="ex: how many employees are there?"
          />
        </b-field>
      </div>
    </div>
    

    <div class="buttons">
      <b-button @click="onGenerate">
        Generate GraphQL Query
      </b-button>
      <b-button @click="onQuery">
        Send Query
      </b-button>
    </div>
    <hr>
    <div class="columns">
      <div class="column">
        <h3>Predicted Query</h3>
        <p>{{ prediction }}</p>
      </div>
      <div class="column">
        <h2>GraphQL Response</h2>
        <p>{{ graphQLResonse }}</p>
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios'
export default {
  name: 'HelloWorld',
  data() {
    return {
      graphqlURL: 'http://localhost:8080/v1/graphql',
      inferenceURL: 'http://926bab7ea6c1.ngrok.io/predict',
      prompt: 'What is the country of the artist named Enrique?',
      prediction: '',
      graphQLResonse: '',
      schemaId: 'music_1',
      schemaIds: ['climbing', 'student_1', 'college_3', 'insurance_and_eClaims', 'car_1', 'department_management', 'cre_Docs_and_Epenses', 'scientist_1', 'voter_2', 'student_assessment', 'activity_1', 'battle_death', 'manufactory_1', 'products_for_hire', 'tracking_software_problems', 'book_2', 'customers_campaigns_ecommerce', 'local_govt_mdm', 'party_host', 'hr_1', 'store_product', 'customer_deliveries', 'pilot_record', 'browser_web', 'music_2', 'game_1', 'protein_institute', 'match_season', 'farm', 'inn_1', 'solvency_ii', 'customers_and_products_contacts', 'concert_singer', 'body_builder', 'candidate_poll', 'college_1', 'academic', 'bike_1', 'entertainment_awards', 'company_employee', 'ship_mission', 'behavior_monitoring', 'cre_Theme_park', 'scholar', 'employee_hire_evaluation', 'phone_1', 'product_catalog', 'baseball_1', 'museum_visit', 'store_1', 'flight_company', 'club_1', 'county_public_safety', 'storm_record', 'city_record', 'formula_1', 'shop_membership', 'school_bus', 'news_report', 'voter_1', 'aircraft', 'medicine_enzyme_interaction', 'company_1', 'coffee_shop', 'chinook_1', 'railway', 'csu_1', 'cre_Doc_Control_Systems', 'local_govt_in_alabama', 'dorm_1', 'small_bank_1', 'entrepreneur', 'phone_market', 'mountain_photos', 'network_2', 'hospital_1', 'gas_company', 'yelp', 'race_track', 'movie_1', 'roller_coaster', 'flight_2', 'decoration_competition', 'assets_maintenance', 'swimming', 'driving_school', 'cre_Doc_Tracking_DB', 'wedding', 'sakila_1', 'culture_company', 'orchestra', 'wrestler', 'ship_1', 'college_2', 'insurance_policies', 'gymnast', 'riding_club', 'cre_Drama_Workshop_Groups', 'flight_1', 'customers_card_transactions', 'theme_gallery', 'world_1', 'department_store', 'dog_kennels', 'perpetrator', 'document_management', 'restaurants', 'election_representative', 'workshop_paper', 'debate', 'film_rank', 'twitter_1', 'party_people', 'icfp_1', 'e_government', 'device', 'flight_4', 'school_finance', 'machine_repair', 'tracking_grants_for_research', 'student_transcripts_tracking', 'program_share', 'loan_1', 'epinions_1', 'music_1', 'allergy_1', 'real_estate_properties', 'singer', 'train_station', 'wine_1', 'tracking_share_transactions', 'wta_1', 'game_injury', 'network_1', 'geo', 'products_gen_characteristics', 'customers_and_invoices', 'company_office', 'station_weather', 'music_4', 'customers_and_addresses', 'manufacturer', 'local_govt_and_lot', 'tvshow', 'course_teach', 'musical', 'architecture', 'apartment_rentals', 'e_learning', 'sports_competition', 'pets_1', 'school_player', 'customer_complaints', 'university_basketball', 'cre_Doc_Template_Mgt', 'journal_committee', 'cinema', 'performance_attendance', 'election', 'soccer_2', 'imdb', 'soccer_1', 'tracking_orders', 'insurance_fnol', 'poker_player', 'restaurant_1']
      // todo add dev schema Ids. 
    }
  }, 
  methods: {
    onGenerate: async function() {
      // send prompt to server
      const req = { prompt: this.prompt, schemaId: this.schemaId}
      const response = await axios.post(this.inferenceURL,req)
      this.prediction = response.data.prediction
    },
    onQuery: async function() {
      const req = { query: this.prediction }
      const response = await axios.post(this.graphqlURL, req)
      this.graphQLResonse = response.data
    }
  }
}
</script>
