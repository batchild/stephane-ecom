    <template>
      <div class="list">
        <div v-for="(event, i) in events" :key="i" class="mb-3">
          <figure
            class="md:flex bg-gray-100 rounded-xl p-8 md:p-0 dark:bg-gray-800"
          >
            <img
              class="w-24 h-24 md:w-48 md:h-auto md:rounded-none rounded-full mx-auto"
              :src="`http://localhost:1337${event.attributes.image.data.attributes.formats.large.url}`"
              alt=""
              width="384"
              height="512"
            />
            <div class="pt-6 md:p-8 text-center md:text-left space-y-4">
              <blockquote>
                <h1 class="text-xl md:text-2xl mb-3 font-bold uppercase">
                  {{ event.attributes.name }}
                </h1>
                <p class="text-sm md:text-lg font-medium">
                  Lorem ipsum dolor sit amet consectetur, adipisicing elit. Debitis
                  dolore dignissimos exercitationem, optio corrupti nihil veniam
                  quod unde reprehenderit cum accusantium quaerat nostrum placeat,
                  sapiente tempore perspiciatis maiores iure esse?
                </p>
              </blockquote>
              <figcaption class="font-medium">
                <div class="text-gray-700 dark:text-gray-500">
                  tickets available: {{ event.attributes.tickets_available == 0 ? 'sold out' : event.attributes.tickets_available }}
                </div>
                <div class="text-gray-700 dark:text-gray-500">
                  {{ formatDate(event.attributes.date) }}
                </div>
              </figcaption>
              <!-- <router-link to="/about"> -->
              <button :disabled=" event.attributes.tickets_available == 0 " @click="getDetail(event.id)" class="bg-black text-white p-3">
                Get tickets
              </button>
              <!-- </router-link> -->
            </div>
          </figure>
        </div>
      </div>
    </template>
    <script>
    import axios from "axios";
    export default {
      data() {
        return {
          events: [],
        };
      },
      methods: {
        getDetail(id) {
          console.log("btn clicked");
          this.$router.push(`/event/${id}`);
        },
        formatDate(date) {
          const timeArr = new Date(date).toLocaleTimeString().split(":");
          const DorN = timeArr.pop().split(" ")[1];
          return `${new Date(date).toDateString()} ${timeArr.join(":")} ${DorN}`;
        },
      },
      async created() {
        const res = await axios.get("http://localhost:1337/api/events?populate=*");
        this.events = res.data.data;
      },
    };
    </script>
    <style scoped></style>