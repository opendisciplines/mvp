<template>
  <iframe src="https://embeds.beehiiv.com/fea2b248-9bcd-4f29-a7e3-e328dc7a6373" data-test-id="beehiiv-embed" width="100%" height="320" frameborder="0" scrolling="no" style="margin: 0; background-color: transparent;"></iframe>
  <div id="app">    
    <div class="table-responsive">
      <table class="table">              
        <thead>
          <tr>
            <th>Name</th>
            <th>Discipline Date</th>
            <th class="summary">Summary</th> <!-- Assign a class to this cell for styling -->
            <th>Spicy Level</th>
            <th>Category</th>
            <th>Province</th> <!-- Added Province column -->
          </tr>
        </thead>
        <tbody>
          <tr v-for="person in people" :key="person.last_name">
            <td>{{ person.first_name }} {{ person.last_name }}</td>
            <td>{{ person.date }}</td>
            <td class="summary">{{ person.summary }}</td> <!-- Assign a class to this cell for styling -->
            <td>{{ getSpicyEmoji(person.spicy_level) }}</td>
            <td>{{ person.category }}</td>
            <td>{{ person.province }}</td> <!-- Added Province cell -->
          </tr>
        </tbody>
      </table>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      people: [],
    };
  },
  methods: {
    getSpicyEmoji(spicyLevel) {
      switch(spicyLevel) {
        case 'Mild':
          return '😮';
        case 'Medium':
          return '😮‍💨';
        case 'Hot':
          return '🥵';
        case 'Extra Hot':
          return '🤯';
        default:
          return spicyLevel; 
      }
    }
  },
  async mounted() {
    try {
      const response = await fetch('/data.json');
      if (!response.ok) {
        throw new Error('Network response was not ok' + response.statusText);
      }
      const data = await response.json();
      this.people = data.sort((a, b) => {
        const dateA = new Date(a.date);
        const dateB = new Date(b.date);
        return dateB - dateA; 
      });
    } catch (error) {
      console.error('There has been a problem with your fetch operation:', error);
    }
  }
};
</script>

<style>
/* Add your styles here */
.table-full-width {
  width: 100%;
}

.summary {
  max-width: 2000px; /* Set a max-width for the summary cell */
  white-space: normal; /* Break words properly */
}

/* Responsive table styling (optional but recommended) */
.table-responsive {
  display: block;
  width: 100%;
  overflow-x: auto;
  -webkit-overflow-scrolling: touch;
  padding-left: 10px;
  padding-right: 10px;
}
</style>
