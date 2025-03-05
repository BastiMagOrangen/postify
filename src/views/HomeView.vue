<script setup></script>

<template>
  <v-container>
    <v-row>
      <v-col cols="12">
        <h1>API Request</h1>
      </v-col>
    </v-row>
    <v-row>
      <v-col cols="2">
        <v-select
          v-model="method"
          :items="methods"
          label="Method"
          outlined
        ></v-select>
      </v-col>
      <v-col cols="8">
        <v-text-field
          v-model="url"
          label="URL"
          outlined
          @keyup.enter="sendRequest"
        ></v-text-field>
      </v-col>
      <v-col cols="2">
        <v-btn color="primary" @click="sendRequest">Send</v-btn>
      </v-col>
    </v-row>

    <!-- Request Body -->
    <v-row>
      <v-col cols="12">
        <v-tabs v-model="activeTab">
          <v-tab>Body</v-tab>
          <v-tab>Headers</v-tab>
          <v-tab-item>
            <v-textarea
              v-model="body"
              label="Request Body (JSON)"
              outlined
              rows="5"
            ></v-textarea>
          </v-tab-item>
          <v-tab-item>
            <v-textarea
              v-model="headers"
              label="Headers (JSON)"
              outlined
              rows="5"
            ></v-textarea>
          </v-tab-item>
        </v-tabs>
      </v-col>
    </v-row>

    <!-- Response -->
    <v-row v-if="response">
      <v-col cols="12">
        <h3>Response</h3>
        <v-card>
          <v-card-text>
            <pre>{{ response }}</pre>
          </v-card-text>
        </v-card>
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
export default {
  name: "Home",
  data: () => ({
    method: "GET",
    methods: ["GET", "POST", "PUT", "DELETE"],
    url: "",
    body: "",
    headers: "",
    activeTab: 0,
    response: null,
  }),
  methods: {
    async sendRequest() {
      try {
        const config = {
          method: this.method,
          url: this.url.startsWith("http") ? this.url : `/api${this.url}`,
          headers: this.headers ? JSON.parse(this.headers) : {},
          data: this.body ? JSON.parse(this.body) : undefined,
        };
        console.log("Request config:", config);

        const res = await fetch(config.url, {
          method: config.method,
          headers: config.headers,
          body:
            config.method !== "GET" ? JSON.stringify(config.data) : undefined,
        });

        console.log("Response status:", res.status); // Statuscode prüfen
        console.log("Response ok:", res.ok); // Erfolgreich?

        // Rohdaten vor dem Parsen prüfen
        const text = await res.text();
        console.log("Raw response:", text);

        // Nur parsen, wenn es Daten gibt
        this.response = text ? JSON.parse(text) : { error: "Empty response" };
      } catch (error) {
        console.error("Fetch error:", error);
        this.response = { error: error.message };
      }
    },
  },
};
</script>
