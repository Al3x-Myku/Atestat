<template>
<div class="app-container">
  <h1>Facy</h1>
  <div class="fancy-upload">
    <form @submit.prevent="uploadImage">
      <label class="custom-file-upload">
        <input type="file" accept="image/jpeg" @change="handleFileChange">
        <span>Select an Image</span>
      </label>
      <button type="submit" class="upload-button" :disabled="isImageValid == false">Upload Image</button>
    </form>

    <div v-if="imageAdded && isImageValid">
      <img :src="imageURL" alt="Uploaded Image" class="uploaded-image" />
    </div>

    <div v-else-if="imageAdded">
      <h2>Image size too big</h2>
    </div>

    <div class="fancy-progress-bar" v-if="gotResponse && imageUploaded">
      <div>
        <label for="progress">Age</label><br>
        <progress id="progress" :value="progressValues[0]" max="100"></progress>
        <div class="progress-value">{{ progressValues[0] }}</div>

        <label for="progress">Angry</label><br>
        <progress id="progress" :value="progressValues[1]" max="100"></progress>
        <div class="progress-value">{{ progressValues[1] }}%</div>

        <label for="progress">Disgust</label><br>
        <progress id="progress" :value="progressValues[2]" max="100"></progress>
        <div class="progress-value">{{ progressValues[2] }}%</div>

        <label for="progress">Fear</label><br>
        <progress id="progress" :value="progressValues[3]" max="100"></progress>
        <div class="progress-value">{{ progressValues[3] }}%</div>
      </div>

      <div>
        <label for="progress">Happy</label><br>
        <progress id="progress" :value="progressValues[4]" max="100"></progress>
        <div class="progress-value">{{ progressValues[4] }}%</div>

        <label for="progress">Neutral</label><br>
        <progress id="progress" :value="progressValues[5]" max="100"></progress>
        <div class="progress-value">{{ progressValues[5] }}%</div>

        <label for="progress">Sad</label><br>
        <progress id="progress" :value="progressValues[6]" max="100"></progress>
        <div class="progress-value">{{ progressValues[6] }}%</div>

        <label for="progress">Surprise</label><br>
        <progress id="progress" :value="progressValues[7]" max="100"></progress>
        <div class="progress-value">{{ progressValues[7] }}%</div>
      </div>
    </div>

    <div class="loading-spinner" v-else-if="loading">
      <h2>Analysing...</h2>
      <div class="spinner"></div>
    </div>

  </div>
</div>
</template>

<script lang="ts">
import axios from 'axios';

interface Analysis {
  age: number,
  angry: number,
  disgust: number,
  fear: number,
  happy: number,
  neutral: number,
  sad: number,
  surprise: number
}

export default {
  data() {
    return {  
      selectedFile: null as File | null,
      isImageValid: false,
      imageAdded: false,
      imageUploaded: false,
      loading: false,
      imageURL: '',
      progressValues: [0, 0, 0, 0, 0, 0, 0, 0],
      gotResponse: false,
      imageID: '',
      data: null as Analysis | null,
    };
  },
  methods: {
    handleFileChange(event: any) {
      this.selectedFile = event.target.files[0];
      this.imageURL = URL.createObjectURL(event.target.files[0]);
      this.imageAdded = true;
      this.imageUploaded = false;
      this.validateImage();
    },
    async uploadImage() {
      console.log('uploading image');
      if (this.selectedFile) {
        this.imageUploaded = true;
        this.gotResponse = false;
        const formData = new FormData();
        formData.append('image', this.selectedFile);
        
        axios.post('http://localhost:5000/api/sentiment_analysis_image', formData, 
        {
          headers: {
            'Content-Type': 'multipart/form-data'
          },
        })
        .then(async res => {
          console.log(res.data);
          this.loading = true;
          this.imageID = res.data['task_id'];
          let status = false;
          const apiString = 'http://localhost:5000/api/get_data_from_image_process?task_id=' + this.imageID; 
          while (status == false) {
            axios.get(apiString)
              .then((res) => {
                  if (res.data['status'] == true){
                    this.data = res.data['data'];
                    console.log('data is', this.data);
                    status = true;
                    this.loading = false;
                    this.gotResponse = true;
                    console.log('got here after first get request, data is', this.data);
                    console.log(this.selectedFile);
                    this.startProgress();
                  }
              })
              .catch((err) => {
                console.log(err);
              });
              await this.sleep(1000 * 2);
            }})
        .catch(err => {
          console.log(err);
        });
      }
    },
    startProgress() {
      this.progressValues = [0, 0, 0, 0, 0, 0, 0, 0];
      console.log('data in startProgress', this.data);
      if (this.data != null) {
        const intervalForAge = setInterval(() => {
          if (this.progressValues[0] < this.data.age) {
            this.progressValues[0]++;
          } else {
            clearInterval(intervalForAge);
          }
        }, 20);
        const intervalForAngry = setInterval(() => {
          if (this.progressValues[1] < this.data.angry) {
            this.progressValues[1]++;
          } else {
            clearInterval(intervalForAngry);
          }
        }, 20);
        const intervalForDisgust = setInterval(() => {
          if (this.progressValues[2] < this.data.disgust) {
            this.progressValues[2]++;
          } else {
            clearInterval(intervalForDisgust);
          }
        }, 20);
        const intervalForFear = setInterval(() => {
          if (this.progressValues[3] < this.data.fear) {
            this.progressValues[3]++;
          } else {
            clearInterval(intervalForFear);
          }
        }, 20);
        const intervalForHappy = setInterval(() => {
          if (this.progressValues[4] < this.data.happy) {
            this.progressValues[4]++;
          } else {
            clearInterval(intervalForHappy);
          }
        }, 20);
        const intervalForNeutral = setInterval(() => {
          if (this.progressValues[5] < this.data.neutral) {
            this.progressValues[5]++;
          } else {
            clearInterval(intervalForNeutral);
          }
        }, 20);
        const intervalForSad = setInterval(() => {
          if (this.progressValues[6] < this.data.sad) {
            this.progressValues[6]++;
          } else {
            clearInterval(intervalForSad);
          }
        }, 20);
        const intervalForSurprise = setInterval(() => {
          if (this.progressValues[7] < this.data.surprise) {
            this.progressValues[7]++;
          } else {
            clearInterval(intervalForSurprise);
          }
        }, 20);
      }
    },
    validateImage() {
      if (!this.selectedFile) {
        this.isImageValid = false;
        return;
      }
      const image = new Image();
      image.src = this.imageURL;

      image.onload = () => {
        const maxWidth = 400;
        const maxHeight = 400;

        if (image.width <= maxWidth && image.height <= maxHeight) {
          console.log('Image has correct dimensions');
          this.isImageValid = true;
        } else {
          console.log('Image has incorrect dimensions');
          this.isImageValid = false;
        }
      };
    },
    sleep(ms: number): Promise<void> {
      return new Promise(resolve => setTimeout(resolve, ms));
    }
  },
};
</script>

<style scoped>
.app-container {
  min-height: 100vh;
  display: flex;
  flex-direction: column;
  overflow: auto;
}

.fancy-upload {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  height: 100vh;
}

.custom-file-upload {
  display: inline-block;
  background-color: turquoise;
  border: 1px solid #ddd;
  border-radius: 4px;
  padding: 6px 12px;
  cursor: pointer;
  transition: background-color 0.3s ease;
}

.custom-file-upload:hover {
  background-color: #e0e0e0;
}

.custom-file-upload input[type="file"] {
  display: none;
}

.upload-button {
  margin-top: 10px;
  padding: 10px 20px;
  background-color: #007bff;
  border: none;
  border-radius: 4px;
  color: white;
  font-weight: bold;
  cursor: pointer;
  transition: background-color 0.3s ease;
}

.upload-button:hover {
  background-color: #0056b3;
}

@media (max-width: 768px) {
  .uploaded-image {
    width: 200%;
  }
}

.fancy-progress-bar {
  margin-top: 20px;
  display: flex;
  justify-content: space-between;
  gap: 20px;
}

#progress {
  width: 200px;
  height: 5%;
  border: none;
  border-radius: 15px;
  background-color: #f3f3f3;
  box-shadow: inset 0 1px 3px rgba(0, 0, 0, 0.1);
  align-items: center;
}

#progress::-webkit-progress-bar {
  background-color: #f3f3f3;
  border-radius: 10px;
}

#progress::-webkit-progress-value {
  background-color: blue;
  border-radius: 10px;
}

.loading-spinner {
  display: flex;
  justify-content: center;
  align-items: center;
  height: 20%; 
}

.spinner {
  border: 4px solid rgba(0, 0, 0, 0.1);
  border-top: 4px solid black; /* Loading spinner color */
  border-radius: 50%;
  width: 40px;
  height: 40px;
  animation: spin 1s linear infinite;
}

@keyframes spin {
  0% { transform: rotate(0deg); }
  100% { transform: rotate(360deg); }
}

.progress-value {
  margin-left: 5px;
  font-weight: bold;
  color: blue;
  animation: bounce 0.5s infinite alternate;
}
</style>
