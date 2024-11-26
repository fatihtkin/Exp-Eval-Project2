<template>
    <div class="demographics-form">
      <div class="form-container">
        <h2>Participant Demographics</h2>
        <form @submit.prevent="submitForm">
          <!-- Age -->
          <label>
            Age:
            <input type="number" v-model="formData.age" required />
          </label>
  
          <!-- Gender -->
          <label>
            Gender:
            <select v-model="formData.gender" required>
              <option disabled value="">Select your gender</option>
              <option>Male</option>
              <option>Female</option>
              <option>Non-binary</option>
              <option>Prefer not to say</option>
            </select>
          </label>
  
          <!-- Programming Experience -->
          <label>
            Programming Experience (years):
            <input type="number" v-model="formData.experience" required />
          </label>
  
          <!-- Preferred Programming Style -->
          <label>
            Preferred Identifier Style:
            <select v-model="formData.stylePreference" required>
              <option disabled value="">Select your preference</option>
              <option>camelCase</option>
              <option>kebab-case</option>
              <option>No preference</option>
            </select>
          </label>
  
          <!-- Educational Background -->
          <label>
            Educational Background:
            <input type="text" v-model="formData.education" placeholder="e.g., Computer Science" />
          </label>
  
          <!-- Work Experience -->
          <label>
            Work Experience (years):
            <input type="number" v-model="formData.workExperience" required />
          </label>
  
          <!-- Favorite Programming Language -->
          <label>
            Favorite Programming Language:
            <input type="text" v-model="formData.favoriteLanguage" placeholder="e.g., JavaScript" />
          </label>
  
          <!-- Submit Button -->
          <button type="submit">Submit</button>
        </form>
      </div>
    </div>
  </template>
  

  <script>
export default {
  name: 'DemographicsForm',
  data() {
    return {
      formData: {
        age: '',
        gender: '',
        experience: '',
        stylePreference: '',
        education: '',
        workExperience: '',
        favoriteLanguage: '',
      },
    };
  },
  methods: {
    submitForm() {
      // Emit the collected form data to the parent component
      this.$emit('form-submitted', this.formData);
    },
  },
};
</script>

<style scoped>
.demographics-form {
  display: flex;
  justify-content: center;
  align-items: center;
  min-height: 100vh;
  background: linear-gradient(135deg, #667eea, #764ba2);
  font-family: 'Arial', sans-serif;
  color: white;
}

.form-container {
  background: white;
  color: #333;
  max-width: 500px;
  padding: 20px 30px;
  border-radius: 15px;
  box-shadow: 0 8px 20px rgba(0, 0, 0, 0.2);
  text-align: center;
}

h2 {
  font-size: 2rem;
  margin-bottom: 1rem;
  color: #764ba2;
}

label {
  display: block;
  text-align: left;
  margin-bottom: 15px;
  font-size: 1rem;
}

input,
select {
  width: 100%;
  padding: 8px;
  margin-top: 5px;
  font-size: 1rem;
  border: 1px solid #ccc;
  border-radius: 5px;
  outline: none;
}

input::placeholder {
  font-style: italic;
}

button {
  background-color: #764ba2;
  color: white;
  font-size: 1rem;
  padding: 10px 20px;
  border: none;
  border-radius: 5px;
  cursor: pointer;
  transition: background-color 0.3s ease;
}

button:hover {
  background-color: #5a3f8a;
}
</style>
