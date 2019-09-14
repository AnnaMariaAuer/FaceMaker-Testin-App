<template>
  <div class="hello">
    <div class="entry">
    <h1>Welcome to our FaceMaker<sup>TM</sup> Testing App</h1>
    <br>
    <p>We've trained a neural network in order to generate realistic faces 
      out of FaceMaker<sup>TM</sup> faces. Although only little changes can be seen, we'd like
      you to rank the following three faces in terms of realistic looking.</p>
    </div>

    <div class="explanation" v-if="counter < counter_limit">  
    <h3>This i how it works:</h3>
    <p>You will be presented various rows of faces each containing three different pictures of the same face.
       Use the radio-buttons to rank the images.</p>
    <ul>
      <li> <strong>Number 1:</strong> This picture looks the most realistic. </li>
      <li> <strong>Number 2:</strong> This picture looks less realistic. </li>
      <li> <strong>Number 3:</strong> This picture looks the least realistic. </li>
    </ul>
    <br>
    <strong>You can use each number only once!</strong>
    </div>
    <div class="user-id-input" v-if="counter < counter_limit">
    <p>Your test manager will enter your User-ID:</p>
    <input v-model="user_id" placeholder="Your User-ID">
    <p>Your User-ID is: {{ user_id }}</p>
    </div>

    <div class="image-container" id="image_container" v-if="counter < counter_limit">
      <!--<div v-bind:key="image.id" v-for="image in images_d1" class="dataset-1">-->
      <div class="dataset-1">
        <!--<img v-for="img in images_d1" v-bind:src="img" v-bind:key="img.id"/>-->
        <img v-bind:src="selectedImage_d1"/>
        <br>
        <section>
          <div class="block">
            <b-radio v-model="radio_d1" 
              name="name"
              native-value="1"
              >
              1
            </b-radio>
            <b-radio v-model="radio_d1" 
              name="name"
              native-value="2"
              >
              2
            </b-radio>
            <b-radio v-model="radio_d1" 
              name="name"
              native-value="3"
              >
              3
            </b-radio>
          </div>
          <p class="content">
            <b>You ranked this image:</b>
             {{ radio_d1 }}
           </p>
        </section>
      </div>
      <div class="dataset-2">
        <img v-bind:src="selectedImage_d2"/>
        <br>
        <section>
          <div class="block">
            <b-radio v-model="radio_d2" v-bind:disabled="radio_d1 === '1'? true : false"
              name="name"
              native-value="1">
              1
            </b-radio>
            <b-radio v-model="radio_d2" v-bind:disabled="radio_d1 === '2' ? true : false" 
              name="name"
              native-value="2">
              2
            </b-radio>
            <b-radio v-model="radio_d2" v-bind:disabled="radio_d1 === '3'  ? true : false"
              name="name"
              native-value="3">
              3
            </b-radio>
          </div>
          <p class="content">
            <b>You ranked this image:</b>
             {{ radio_d2 }}
           </p>
        </section>
      </div>
      <div class="dataset-3">
        <img v-bind:src="selectedImage_d3"/>
        <br>
        <section>
          <div class="block">
            <b-radio v-model="radio_d3" v-bind:disabled="radio_d1 === '1' || radio_d2 === '1' ? true : false"
              name="name"
              native-value="1">
              1
            </b-radio>
            <b-radio v-model="radio_d3" v-bind:disabled="radio_d1 === '2' || radio_d2 === '2' ? true : false"
              name="name"
              native-value="2">
              2
            </b-radio>
            <b-radio v-model="radio_d3" v-bind:disabled="radio_d1 === '3' || radio_d2 === '3' ? true : false"
              name="name"
              native-value="3">
              3
            </b-radio>
          </div>
          <p class="content">
            <b>You ranked this image:</b>
             {{ radio_d3 }}
           </p>
        </section>
      </div>
    </div>
    <div v-else class="finished">
      <strong>Thank you for taking part in our study!</strong>
      <strong> Please answer the following questionnaire about your demographic data.</strong>
      <p></p>
      <a class="button is primary" href="https://forms.gle/jttdxtN8T4vA77RS9" target="_blank">Questionnaire</a>
    </div>
    <p v-if="counter < counter_limit" id="warning-number" v-bind:style="{display: display_warning_number}">Use each number only one.</p>
    <p v-if="counter < counter_limit" id="warning-ranking" v-bind:style="{display: display_warning_ranking}">Please rank each picture.</p>
    <a v-if="counter < counter_limit" id="button_next" class="button is-primary" @click="saveRatings(); reloadImages()" 
                                                    :disabled=isDisabled>Show me new pictures</a>
  </div>
</template>

<script>
import Image from '../assets/dataset-1/1.png'
export default {
  name: 'HelloWorld',

  data(){
 
    return {
      radio_d1: '',
      radio_d2: '',
      radio_d3: '',
      random_index: 0,
      counter: 0,
      counter_limit: 30, /**Number of images a user is supposed to rank */
      user_id: '',
      random_placeholder: '',
      display_warning_number: 'none',
      display_warning_ranking: 'none',
      button_state: 'disabled',

      images_all: [
        require('@/assets/dataset-1/1.png'),
        require('@/assets/dataset-1/2.png'),
        require('@/assets/dataset-1/3.png'),
        require('@/assets/dataset-1/4.png'),
        require('@/assets/dataset-1/5.png'),
        require('@/assets/dataset-1/6.png'),
        require('@/assets/dataset-1/7.png'),
        require('@/assets/dataset-1/8.png'),
        require('@/assets/dataset-1/9.png'),
        require('@/assets/dataset-1/10.png'),
        require('@/assets/dataset-1/11.png'),
        require('@/assets/dataset-1/12.png'),
        require('@/assets/dataset-1/13.png'),
        require('@/assets/dataset-1/14.png'),
        require('@/assets/dataset-1/15.png'),
        require('@/assets/dataset-1/16.png'),
        require('@/assets/dataset-1/17.png'),
        require('@/assets/dataset-1/18.png'),
        require('@/assets/dataset-1/19.png'),
        require('@/assets/dataset-1/20.png'),
        require('@/assets/dataset-1/21.png'),
        require('@/assets/dataset-1/22.png'),
        require('@/assets/dataset-1/23.png'),
        require('@/assets/dataset-1/24.png'),
        require('@/assets/dataset-1/25.png'),
        require('@/assets/dataset-1/26.png'),
        require('@/assets/dataset-1/27.png'),
        require('@/assets/dataset-1/28.png'),
        require('@/assets/dataset-1/29.png'),
        require('@/assets/dataset-1/30.png'),
        require('@/assets/dataset-1/31.png'),
        require('@/assets/dataset-1/32.png'),
        require('@/assets/dataset-1/33.png'),
        require('@/assets/dataset-1/34.png'),
        require('@/assets/dataset-1/35.png'),
        require('@/assets/dataset-1/36.png'),
        require('@/assets/dataset-1/37.png'),
        require('@/assets/dataset-1/38.png'),
        require('@/assets/dataset-1/39.png'),
        require('@/assets/dataset-1/40.png'),
        require('@/assets/dataset-1/41.png'),
        require('@/assets/dataset-1/42.png'),
        require('@/assets/dataset-1/43.png'),
        require('@/assets/dataset-1/44.png'),
        require('@/assets/dataset-1/45.png'),
        require('@/assets/dataset-1/46.png'),
        require('@/assets/dataset-1/47.png'),
        require('@/assets/dataset-1/48.png'),
        require('@/assets/dataset-1/49.png'),
        require('@/assets/dataset-1/50.png'),
        require('@/assets/dataset-1/51.png'),
        require('@/assets/dataset-1/52.png'),
        require('@/assets/dataset-1/53.png'),
        require('@/assets/dataset-1/54.png'),
        require('@/assets/dataset-1/55.png'),
        require('@/assets/dataset-1/56.png'),
        require('@/assets/dataset-1/57.png'),
        require('@/assets/dataset-1/58.png'),
        require('@/assets/dataset-1/59.png'),
        require('@/assets/dataset-1/60.png'),
        require('@/assets/dataset-1/61.png'),
        require('@/assets/dataset-1/62.png'),
        require('@/assets/dataset-1/63.png'),
        require('@/assets/dataset-1/64.png'),
        require('@/assets/dataset-1/65.png'),
        require('@/assets/dataset-1/66.png'),
        require('@/assets/dataset-1/67.png'),
        require('@/assets/dataset-1/68.png'),
        require('@/assets/dataset-1/69.png'),
        require('@/assets/dataset-1/70.png'),
        require('@/assets/dataset-1/71.png'),
        require('@/assets/dataset-1/72.png'),
        require('@/assets/dataset-1/73.png'),
        require('@/assets/dataset-1/74.png'),
        require('@/assets/dataset-1/75.png'),
        require('@/assets/dataset-1/76.png'),
        require('@/assets/dataset-1/77.png'),
        require('@/assets/dataset-1/78.png'),
        require('@/assets/dataset-1/79.png'),
        require('@/assets/dataset-1/80.png'),
        require('@/assets/dataset-1/81.png'),
        require('@/assets/dataset-1/82.png'),
        require('@/assets/dataset-1/83.png'),
        require('@/assets/dataset-1/84.png'),
        require('@/assets/dataset-1/85.png'),
        require('@/assets/dataset-1/86.png'),
        require('@/assets/dataset-1/87.png'),
        require('@/assets/dataset-1/88.png'),
        require('@/assets/dataset-1/89.png'),
        require('@/assets/dataset-1/90.png'),
        require('@/assets/dataset-1/91.png'),
        require('@/assets/dataset-1/92.png'),
        require('@/assets/dataset-1/93.png'),
        require('@/assets/dataset-1/94.png'),
        require('@/assets/dataset-1/95.png'),
        require('@/assets/dataset-1/96.png'),
        require('@/assets/dataset-1/97.png'),
        require('@/assets/dataset-1/98.png'),
        require('@/assets/dataset-1/99.png'),
        require('@/assets/dataset-1/100.png'),
        require('@/assets/dataset-2/1.png'),
        require('@/assets/dataset-2/2.png'),
        require('@/assets/dataset-2/3.png'),
        require('@/assets/dataset-2/4.png'),
        require('@/assets/dataset-2/5.png'),
        require('@/assets/dataset-2/6.png'),
        require('@/assets/dataset-2/7.png'),
        require('@/assets/dataset-2/8.png'),
        require('@/assets/dataset-2/9.png'),
        require('@/assets/dataset-2/10.png'),
        require('@/assets/dataset-2/11.png'),
        require('@/assets/dataset-2/12.png'),
        require('@/assets/dataset-2/13.png'),
        require('@/assets/dataset-2/14.png'),
        require('@/assets/dataset-2/15.png'),
        require('@/assets/dataset-2/16.png'),
        require('@/assets/dataset-2/17.png'),
        require('@/assets/dataset-2/18.png'),
        require('@/assets/dataset-2/19.png'),
        require('@/assets/dataset-2/20.png'),
        require('@/assets/dataset-2/21.png'),
        require('@/assets/dataset-2/22.png'),
        require('@/assets/dataset-2/23.png'),
        require('@/assets/dataset-2/24.png'),
        require('@/assets/dataset-2/25.png'),
        require('@/assets/dataset-2/26.png'),
        require('@/assets/dataset-2/27.png'),
        require('@/assets/dataset-2/28.png'),
        require('@/assets/dataset-2/29.png'),
        require('@/assets/dataset-2/30.png'),
        require('@/assets/dataset-2/31.png'),
        require('@/assets/dataset-2/32.png'),
        require('@/assets/dataset-2/33.png'),
        require('@/assets/dataset-2/34.png'),
        require('@/assets/dataset-2/35.png'),
        require('@/assets/dataset-2/36.png'),
        require('@/assets/dataset-2/37.png'),
        require('@/assets/dataset-2/38.png'),
        require('@/assets/dataset-2/39.png'),
        require('@/assets/dataset-2/40.png'),
        require('@/assets/dataset-2/41.png'),
        require('@/assets/dataset-2/42.png'),
        require('@/assets/dataset-2/43.png'),
        require('@/assets/dataset-2/44.png'),
        require('@/assets/dataset-2/45.png'),
        require('@/assets/dataset-2/46.png'),
        require('@/assets/dataset-2/47.png'),
        require('@/assets/dataset-2/48.png'),
        require('@/assets/dataset-2/49.png'),
        require('@/assets/dataset-2/50.png'),
        require('@/assets/dataset-2/51.png'),
        require('@/assets/dataset-2/52.png'),
        require('@/assets/dataset-2/53.png'),
        require('@/assets/dataset-2/54.png'),
        require('@/assets/dataset-2/55.png'),
        require('@/assets/dataset-2/56.png'),
        require('@/assets/dataset-2/57.png'),
        require('@/assets/dataset-2/58.png'),
        require('@/assets/dataset-2/59.png'),
        require('@/assets/dataset-2/60.png'),
        require('@/assets/dataset-2/61.png'),
        require('@/assets/dataset-2/62.png'),
        require('@/assets/dataset-2/63.png'),
        require('@/assets/dataset-2/64.png'),
        require('@/assets/dataset-2/65.png'),
        require('@/assets/dataset-2/66.png'),
        require('@/assets/dataset-2/67.png'),
        require('@/assets/dataset-2/68.png'),
        require('@/assets/dataset-2/69.png'),
        require('@/assets/dataset-2/70.png'),
        require('@/assets/dataset-2/71.png'),
        require('@/assets/dataset-2/72.png'),
        require('@/assets/dataset-2/73.png'),
        require('@/assets/dataset-2/74.png'),
        require('@/assets/dataset-2/75.png'),
        require('@/assets/dataset-2/76.png'),
        require('@/assets/dataset-2/77.png'),
        require('@/assets/dataset-2/78.png'),
        require('@/assets/dataset-2/79.png'),
        require('@/assets/dataset-2/80.png'),
        require('@/assets/dataset-2/81.png'),
        require('@/assets/dataset-2/82.png'),
        require('@/assets/dataset-2/83.png'),
        require('@/assets/dataset-2/84.png'),
        require('@/assets/dataset-2/85.png'),
        require('@/assets/dataset-2/86.png'),
        require('@/assets/dataset-2/87.png'),
        require('@/assets/dataset-2/88.png'),
        require('@/assets/dataset-2/89.png'),
        require('@/assets/dataset-2/90.png'),
        require('@/assets/dataset-2/91.png'),
        require('@/assets/dataset-2/92.png'),
        require('@/assets/dataset-2/93.png'),
        require('@/assets/dataset-2/94.png'),
        require('@/assets/dataset-2/95.png'),
        require('@/assets/dataset-2/96.png'),
        require('@/assets/dataset-2/97.png'),
        require('@/assets/dataset-2/98.png'),
        require('@/assets/dataset-2/99.png'),
        require('@/assets/dataset-2/100.png'),
        require('@/assets/dataset-3/1.png'),
        require('@/assets/dataset-3/2.png'),
        require('@/assets/dataset-3/3.png'),
        require('@/assets/dataset-3/4.png'),
        require('@/assets/dataset-3/5.png'),
        require('@/assets/dataset-3/6.png'),
        require('@/assets/dataset-3/7.png'),
        require('@/assets/dataset-3/8.png'),
        require('@/assets/dataset-3/9.png'),
        require('@/assets/dataset-3/10.png'),
        require('@/assets/dataset-3/11.png'),
        require('@/assets/dataset-3/12.png'),
        require('@/assets/dataset-3/13.png'),
        require('@/assets/dataset-3/14.png'),
        require('@/assets/dataset-3/15.png'),
        require('@/assets/dataset-3/16.png'),
        require('@/assets/dataset-3/17.png'),
        require('@/assets/dataset-3/18.png'),
        require('@/assets/dataset-3/19.png'),
        require('@/assets/dataset-3/20.png'),
        require('@/assets/dataset-3/21.png'),
        require('@/assets/dataset-3/22.png'),
        require('@/assets/dataset-3/23.png'),
        require('@/assets/dataset-3/24.png'),
        require('@/assets/dataset-3/25.png'),
        require('@/assets/dataset-3/26.png'),
        require('@/assets/dataset-3/27.png'),
        require('@/assets/dataset-3/28.png'),
        require('@/assets/dataset-3/29.png'),
        require('@/assets/dataset-3/30.png'),
        require('@/assets/dataset-3/31.png'),
        require('@/assets/dataset-3/32.png'),
        require('@/assets/dataset-3/33.png'),
        require('@/assets/dataset-3/34.png'),
        require('@/assets/dataset-3/35.png'),
        require('@/assets/dataset-3/36.png'),
        require('@/assets/dataset-3/37.png'),
        require('@/assets/dataset-3/38.png'),
        require('@/assets/dataset-3/39.png'),
        require('@/assets/dataset-3/40.png'),
        require('@/assets/dataset-3/41.png'),
        require('@/assets/dataset-3/42.png'),
        require('@/assets/dataset-3/43.png'),
        require('@/assets/dataset-3/44.png'),
        require('@/assets/dataset-3/45.png'),
        require('@/assets/dataset-3/46.png'),
        require('@/assets/dataset-3/47.png'),
        require('@/assets/dataset-3/48.png'),
        require('@/assets/dataset-3/49.png'),
        require('@/assets/dataset-3/50.png'),
        require('@/assets/dataset-3/51.png'),
        require('@/assets/dataset-3/52.png'),
        require('@/assets/dataset-3/53.png'),
        require('@/assets/dataset-3/54.png'),
        require('@/assets/dataset-3/55.png'),
        require('@/assets/dataset-3/56.png'),
        require('@/assets/dataset-3/57.png'),
        require('@/assets/dataset-3/58.png'),
        require('@/assets/dataset-3/59.png'),
        require('@/assets/dataset-3/60.png'),
        require('@/assets/dataset-3/61.png'),
        require('@/assets/dataset-3/62.png'),
        require('@/assets/dataset-3/63.png'),
        require('@/assets/dataset-3/64.png'),
        require('@/assets/dataset-3/65.png'),
        require('@/assets/dataset-3/66.png'),
        require('@/assets/dataset-3/67.png'),
        require('@/assets/dataset-3/68.png'),
        require('@/assets/dataset-3/69.png'),
        require('@/assets/dataset-3/70.png'),
        require('@/assets/dataset-3/71.png'),
        require('@/assets/dataset-3/72.png'),
        require('@/assets/dataset-3/73.png'),
        require('@/assets/dataset-3/74.png'),
        require('@/assets/dataset-3/75.png'),
        require('@/assets/dataset-3/76.png'),
        require('@/assets/dataset-3/77.png'),
        require('@/assets/dataset-3/78.png'),
        require('@/assets/dataset-3/79.png'),
        require('@/assets/dataset-3/80.png'),
        require('@/assets/dataset-3/81.png'),
        require('@/assets/dataset-3/82.png'),
        require('@/assets/dataset-3/83.png'),
        require('@/assets/dataset-3/84.png'),
        require('@/assets/dataset-3/85.png'),
        require('@/assets/dataset-3/86.png'),
        require('@/assets/dataset-3/87.png'),
        require('@/assets/dataset-3/88.png'),
        require('@/assets/dataset-3/89.png'),
        require('@/assets/dataset-3/90.png'),
        require('@/assets/dataset-3/91.png'),
        require('@/assets/dataset-3/92.png'),
        require('@/assets/dataset-3/93.png'),
        require('@/assets/dataset-3/94.png'),
        require('@/assets/dataset-3/95.png'),
        require('@/assets/dataset-3/96.png'),
        require('@/assets/dataset-3/97.png'),
        require('@/assets/dataset-3/98.png'),
        require('@/assets/dataset-3/99.png'),
        require('@/assets/dataset-3/100.png'),

      ],
      
      images_d1: [
        require('@/assets/dataset-1/1.png'),
        require('@/assets/dataset-1/2.png'),
        require('@/assets/dataset-1/3.png'),
        require('@/assets/dataset-1/4.png'),
        require('@/assets/dataset-1/5.png'),
        require('@/assets/dataset-1/6.png'),
        require('@/assets/dataset-1/7.png'),
        require('@/assets/dataset-1/8.png'),
        require('@/assets/dataset-1/9.png'),
        require('@/assets/dataset-1/10.png'),
        require('@/assets/dataset-1/11.png'),
        require('@/assets/dataset-1/12.png'),
        require('@/assets/dataset-1/13.png'),
        require('@/assets/dataset-1/14.png'),
        require('@/assets/dataset-1/15.png'),
        require('@/assets/dataset-1/16.png'),
        require('@/assets/dataset-1/17.png'),
        require('@/assets/dataset-1/18.png'),
        require('@/assets/dataset-1/19.png'),
        require('@/assets/dataset-1/20.png'),
        require('@/assets/dataset-1/21.png'),
        require('@/assets/dataset-1/22.png'),
        require('@/assets/dataset-1/23.png'),
        require('@/assets/dataset-1/24.png'),
        require('@/assets/dataset-1/25.png'),
        require('@/assets/dataset-1/26.png'),
        require('@/assets/dataset-1/27.png'),
        require('@/assets/dataset-1/28.png'),
        require('@/assets/dataset-1/29.png'),
        require('@/assets/dataset-1/30.png'),
        require('@/assets/dataset-1/31.png'),
        require('@/assets/dataset-1/32.png'),
        require('@/assets/dataset-1/33.png'),
        require('@/assets/dataset-1/34.png'),
        require('@/assets/dataset-1/35.png'),
        require('@/assets/dataset-1/36.png'),
        require('@/assets/dataset-1/37.png'),
        require('@/assets/dataset-1/38.png'),
        require('@/assets/dataset-1/39.png'),
        require('@/assets/dataset-1/40.png'),
        require('@/assets/dataset-1/41.png'),
        require('@/assets/dataset-1/42.png'),
        require('@/assets/dataset-1/43.png'),
        require('@/assets/dataset-1/44.png'),
        require('@/assets/dataset-1/45.png'),
        require('@/assets/dataset-1/46.png'),
        require('@/assets/dataset-1/47.png'),
        require('@/assets/dataset-1/48.png'),
        require('@/assets/dataset-1/49.png'),
        require('@/assets/dataset-1/50.png'),
        require('@/assets/dataset-1/51.png'),
        require('@/assets/dataset-1/52.png'),
        require('@/assets/dataset-1/53.png'),
        require('@/assets/dataset-1/54.png'),
        require('@/assets/dataset-1/55.png'),
        require('@/assets/dataset-1/56.png'),
        require('@/assets/dataset-1/57.png'),
        require('@/assets/dataset-1/58.png'),
        require('@/assets/dataset-1/59.png'),
        require('@/assets/dataset-1/60.png'),
        require('@/assets/dataset-1/61.png'),
        require('@/assets/dataset-1/62.png'),
        require('@/assets/dataset-1/63.png'),
        require('@/assets/dataset-1/64.png'),
        require('@/assets/dataset-1/65.png'),
        require('@/assets/dataset-1/66.png'),
        require('@/assets/dataset-1/67.png'),
        require('@/assets/dataset-1/68.png'),
        require('@/assets/dataset-1/69.png'),
        require('@/assets/dataset-1/70.png'),
        require('@/assets/dataset-1/71.png'),
        require('@/assets/dataset-1/72.png'),
        require('@/assets/dataset-1/73.png'),
        require('@/assets/dataset-1/74.png'),
        require('@/assets/dataset-1/75.png'),
        require('@/assets/dataset-1/76.png'),
        require('@/assets/dataset-1/77.png'),
        require('@/assets/dataset-1/78.png'),
        require('@/assets/dataset-1/79.png'),
        require('@/assets/dataset-1/80.png'),
        require('@/assets/dataset-1/81.png'),
        require('@/assets/dataset-1/82.png'),
        require('@/assets/dataset-1/83.png'),
        require('@/assets/dataset-1/84.png'),
        require('@/assets/dataset-1/85.png'),
        require('@/assets/dataset-1/86.png'),
        require('@/assets/dataset-1/87.png'),
        require('@/assets/dataset-1/88.png'),
        require('@/assets/dataset-1/89.png'),
        require('@/assets/dataset-1/90.png'),
        require('@/assets/dataset-1/91.png'),
        require('@/assets/dataset-1/92.png'),
        require('@/assets/dataset-1/93.png'),
        require('@/assets/dataset-1/94.png'),
        require('@/assets/dataset-1/95.png'),
        require('@/assets/dataset-1/96.png'),
        require('@/assets/dataset-1/97.png'),
        require('@/assets/dataset-1/98.png'),
        require('@/assets/dataset-1/99.png'),
        require('@/assets/dataset-1/100.png'),
      ],

      images_d2: [
        require('@/assets/dataset-2/1.png'),
        require('@/assets/dataset-2/2.png'),
        require('@/assets/dataset-2/3.png'),
        require('@/assets/dataset-2/4.png'),
        require('@/assets/dataset-2/5.png'),
        require('@/assets/dataset-2/6.png'),
        require('@/assets/dataset-2/7.png'),
        require('@/assets/dataset-2/8.png'),
        require('@/assets/dataset-2/9.png'),
        require('@/assets/dataset-2/10.png'),
        require('@/assets/dataset-2/11.png'),
        require('@/assets/dataset-2/12.png'),
        require('@/assets/dataset-2/13.png'),
        require('@/assets/dataset-2/14.png'),
        require('@/assets/dataset-2/15.png'),
        require('@/assets/dataset-2/16.png'),
        require('@/assets/dataset-2/17.png'),
        require('@/assets/dataset-2/18.png'),
        require('@/assets/dataset-2/19.png'),
        require('@/assets/dataset-2/20.png'),
        require('@/assets/dataset-2/21.png'),
        require('@/assets/dataset-2/22.png'),
        require('@/assets/dataset-2/23.png'),
        require('@/assets/dataset-2/24.png'),
        require('@/assets/dataset-2/25.png'),
        require('@/assets/dataset-2/26.png'),
        require('@/assets/dataset-2/27.png'),
        require('@/assets/dataset-2/28.png'),
        require('@/assets/dataset-2/29.png'),
        require('@/assets/dataset-2/30.png'),
        require('@/assets/dataset-2/31.png'),
        require('@/assets/dataset-2/32.png'),
        require('@/assets/dataset-2/33.png'),
        require('@/assets/dataset-2/34.png'),
        require('@/assets/dataset-2/35.png'),
        require('@/assets/dataset-2/36.png'),
        require('@/assets/dataset-2/37.png'),
        require('@/assets/dataset-2/38.png'),
        require('@/assets/dataset-2/39.png'),
        require('@/assets/dataset-2/40.png'),
        require('@/assets/dataset-2/41.png'),
        require('@/assets/dataset-2/42.png'),
        require('@/assets/dataset-2/43.png'),
        require('@/assets/dataset-2/44.png'),
        require('@/assets/dataset-2/45.png'),
        require('@/assets/dataset-2/46.png'),
        require('@/assets/dataset-2/47.png'),
        require('@/assets/dataset-2/48.png'),
        require('@/assets/dataset-2/49.png'),
        require('@/assets/dataset-2/50.png'),
        require('@/assets/dataset-2/51.png'),
        require('@/assets/dataset-2/52.png'),
        require('@/assets/dataset-2/53.png'),
        require('@/assets/dataset-2/54.png'),
        require('@/assets/dataset-2/55.png'),
        require('@/assets/dataset-2/56.png'),
        require('@/assets/dataset-2/57.png'),
        require('@/assets/dataset-2/58.png'),
        require('@/assets/dataset-2/59.png'),
        require('@/assets/dataset-2/60.png'),
        require('@/assets/dataset-2/61.png'),
        require('@/assets/dataset-2/62.png'),
        require('@/assets/dataset-2/63.png'),
        require('@/assets/dataset-2/64.png'),
        require('@/assets/dataset-2/65.png'),
        require('@/assets/dataset-2/66.png'),
        require('@/assets/dataset-2/67.png'),
        require('@/assets/dataset-2/68.png'),
        require('@/assets/dataset-2/69.png'),
        require('@/assets/dataset-2/70.png'),
        require('@/assets/dataset-2/71.png'),
        require('@/assets/dataset-2/72.png'),
        require('@/assets/dataset-2/73.png'),
        require('@/assets/dataset-2/74.png'),
        require('@/assets/dataset-2/75.png'),
        require('@/assets/dataset-2/76.png'),
        require('@/assets/dataset-2/77.png'),
        require('@/assets/dataset-2/78.png'),
        require('@/assets/dataset-2/79.png'),
        require('@/assets/dataset-2/80.png'),
        require('@/assets/dataset-2/81.png'),
        require('@/assets/dataset-2/82.png'),
        require('@/assets/dataset-2/83.png'),
        require('@/assets/dataset-2/84.png'),
        require('@/assets/dataset-2/85.png'),
        require('@/assets/dataset-2/86.png'),
        require('@/assets/dataset-2/87.png'),
        require('@/assets/dataset-2/88.png'),
        require('@/assets/dataset-2/89.png'),
        require('@/assets/dataset-2/90.png'),
        require('@/assets/dataset-2/91.png'),
        require('@/assets/dataset-2/92.png'),
        require('@/assets/dataset-2/93.png'),
        require('@/assets/dataset-2/94.png'),
        require('@/assets/dataset-2/95.png'),
        require('@/assets/dataset-2/96.png'),
        require('@/assets/dataset-2/97.png'),
        require('@/assets/dataset-2/98.png'),
        require('@/assets/dataset-2/99.png'),
        require('@/assets/dataset-2/100.png'),
      ],

      images_d3: [
        require('@/assets/dataset-3/1.png'),
        require('@/assets/dataset-3/2.png'),
        require('@/assets/dataset-3/3.png'),
        require('@/assets/dataset-3/4.png'),
        require('@/assets/dataset-3/5.png'),
        require('@/assets/dataset-3/6.png'),
        require('@/assets/dataset-3/7.png'),
        require('@/assets/dataset-3/8.png'),
        require('@/assets/dataset-3/9.png'),
        require('@/assets/dataset-3/10.png'),
        require('@/assets/dataset-3/11.png'),
        require('@/assets/dataset-3/12.png'),
        require('@/assets/dataset-3/13.png'),
        require('@/assets/dataset-3/14.png'),
        require('@/assets/dataset-3/15.png'),
        require('@/assets/dataset-3/16.png'),
        require('@/assets/dataset-3/17.png'),
        require('@/assets/dataset-3/18.png'),
        require('@/assets/dataset-3/19.png'),
        require('@/assets/dataset-3/20.png'),
        require('@/assets/dataset-3/21.png'),
        require('@/assets/dataset-3/22.png'),
        require('@/assets/dataset-3/23.png'),
        require('@/assets/dataset-3/24.png'),
        require('@/assets/dataset-3/25.png'),
        require('@/assets/dataset-3/26.png'),
        require('@/assets/dataset-3/27.png'),
        require('@/assets/dataset-3/28.png'),
        require('@/assets/dataset-3/29.png'),
        require('@/assets/dataset-3/30.png'),
        require('@/assets/dataset-3/31.png'),
        require('@/assets/dataset-3/32.png'),
        require('@/assets/dataset-3/33.png'),
        require('@/assets/dataset-3/34.png'),
        require('@/assets/dataset-3/35.png'),
        require('@/assets/dataset-3/36.png'),
        require('@/assets/dataset-3/37.png'),
        require('@/assets/dataset-3/38.png'),
        require('@/assets/dataset-3/39.png'),
        require('@/assets/dataset-3/40.png'),
        require('@/assets/dataset-3/41.png'),
        require('@/assets/dataset-3/42.png'),
        require('@/assets/dataset-3/43.png'),
        require('@/assets/dataset-3/44.png'),
        require('@/assets/dataset-3/45.png'),
        require('@/assets/dataset-3/46.png'),
        require('@/assets/dataset-3/47.png'),
        require('@/assets/dataset-3/48.png'),
        require('@/assets/dataset-3/49.png'),
        require('@/assets/dataset-3/50.png'),
        require('@/assets/dataset-3/51.png'),
        require('@/assets/dataset-3/52.png'),
        require('@/assets/dataset-3/53.png'),
        require('@/assets/dataset-3/54.png'),
        require('@/assets/dataset-3/55.png'),
        require('@/assets/dataset-3/56.png'),
        require('@/assets/dataset-3/57.png'),
        require('@/assets/dataset-3/58.png'),
        require('@/assets/dataset-3/59.png'),
        require('@/assets/dataset-3/60.png'),
        require('@/assets/dataset-3/61.png'),
        require('@/assets/dataset-3/62.png'),
        require('@/assets/dataset-3/63.png'),
        require('@/assets/dataset-3/64.png'),
        require('@/assets/dataset-3/65.png'),
        require('@/assets/dataset-3/66.png'),
        require('@/assets/dataset-3/67.png'),
        require('@/assets/dataset-3/68.png'),
        require('@/assets/dataset-3/69.png'),
        require('@/assets/dataset-3/70.png'),
        require('@/assets/dataset-3/71.png'),
        require('@/assets/dataset-3/72.png'),
        require('@/assets/dataset-3/73.png'),
        require('@/assets/dataset-3/74.png'),
        require('@/assets/dataset-3/75.png'),
        require('@/assets/dataset-3/76.png'),
        require('@/assets/dataset-3/77.png'),
        require('@/assets/dataset-3/78.png'),
        require('@/assets/dataset-3/79.png'),
        require('@/assets/dataset-3/80.png'),
        require('@/assets/dataset-3/81.png'),
        require('@/assets/dataset-3/82.png'),
        require('@/assets/dataset-3/83.png'),
        require('@/assets/dataset-3/84.png'),
        require('@/assets/dataset-3/85.png'),
        require('@/assets/dataset-3/86.png'),
        require('@/assets/dataset-3/87.png'),
        require('@/assets/dataset-3/88.png'),
        require('@/assets/dataset-3/89.png'),
        require('@/assets/dataset-3/90.png'),
        require('@/assets/dataset-3/91.png'),
        require('@/assets/dataset-3/92.png'),
        require('@/assets/dataset-3/93.png'),
        require('@/assets/dataset-3/94.png'),
        require('@/assets/dataset-3/95.png'),
        require('@/assets/dataset-3/96.png'),
        require('@/assets/dataset-3/97.png'),
        require('@/assets/dataset-3/98.png'),
        require('@/assets/dataset-3/99.png'),
        require('@/assets/dataset-3/100.png'),
      ],

      arr: [],

      selectedImage_d1: '',
      selectedImage_d2: '',
      selectedImage_d3: '',

      leftImage: '',
      midImage: '',
      rightImage: ''
    }
  },

  created() {
    /** When refreshing or opening the page, random images are set */
    const idx = Math.floor(Math.random() * this.images_d1.length);
    const shuffle_place = Math.floor(Math.random() * 3);
    this.random_index = idx;
    this.random_placeholder = shuffle_place;
 
      if (shuffle_place == 0){
        this.selectedImage_d1 = this.images_all[idx];
        this.selectedImage_d2 = this.images_all[idx+100];
        this.selectedImage_d3 = this.images_all[idx+200];
        this.leftImage = 'dataset1';
        this.midImage = 'dataset2';
        this.rightImage = 'dataset3';
      }

      else if (shuffle_place == 1){
        this.selectedImage_d1 = this.images_all[idx+200];
        this.selectedImage_d2 = this.images_all[idx];
        this.selectedImage_d3 = this.images_all[idx+100];
        this.leftImage = 'dataset3';
        this.midImage = 'dataset1';
        this.rightImage = 'dataset2';
      }

      else {
        this.selectedImage_d1 = this.images_all[idx+100];
        this.selectedImage_d2 = this.images_all[idx+200];
        this.selectedImage_d3 = this.images_all[idx];
        this.leftImage = 'dataset2';
        this.midImage = 'dataset3';
        this.rightImage = 'dataset1';
      }
  },

  methods: {    
    saveRatings() {
      if (this.button_state === 'disabled') {
        event.preventDefault();
      }

      else {
        this.counter += 1;
        
        if (this.random_placeholder == '0'){
            this.arr.push({user_id:this.user_id, image_id: this.random_index, dataset_1_ranking: this.radio_d1, dataset_2_ranking: this.radio_d2,
                        dataset_3_ranking: this.radio_d3});
        }

        else if (this.random_placeholder == '1') {
            this.arr.push({user_id:this.user_id, image_id: this.random_index, dataset_1_ranking: this.radio_d2, dataset_2_ranking: this.radio_d3,
                        dataset_3_ranking: this.radio_d1});
        }

        else if (this.random_placeholder == '2'){
            this.arr.push({user_id:this.user_id, image_id: this.random_index, dataset_1_ranking: this.radio_d3, dataset_2_ranking: this.radio_d1,
                        dataset_3_ranking: this.radio_d2});
        }

        const data = JSON.stringify(this.arr);
        window.localStorage.setItem('arr', data);
        console.log(JSON.parse(window.localStorage.getItem('arr')))

       /**Downloads json-File when a testperson is finished */
        if (this.counter==this.counter_limit){
          const blob = new Blob([data], {type: 'text/plain'})
          const e = document.createEvent('MouseEvents'),
          a = document.createElement('a');
          a.download = "user_" + this.user_id.toString() + "_ratings.json";
          a.href = window.URL.createObjectURL(blob);
          a.dataset.downloadurl = ['text/json', a.download, a.href].join(':');
          e.initEvent('click', true, false, window, 0, 0, 0, 0, 0, false, false, false, false, 0, null);
          a.dispatchEvent(e);
      }

    }
    },

    reloadImages(){
      /** When clicking on button, a new row of images is presented*/
      if (this.button_state === 'disabled') {
        event.preventDefault();
      }
      else {
        const idx = Math.floor(Math.random() * this.images_d1.length);
        const shuffle_place = Math.floor(Math.random() * 3);
        this.random_index = idx;
        this.random_placeholder = shuffle_place;
 
        if (shuffle_place == 0){
          this.selectedImage_d1 = this.images_all[idx];
          this.selectedImage_d2 = this.images_all[idx+100];
          this.selectedImage_d3 = this.images_all[idx+200];
          this.leftImage = 'dataset1';
          this.midImage = 'dataset2';
          this.rightImage = 'dataset3';
        }

        else if (shuffle_place == 1){
          this.selectedImage_d1 = this.images_all[idx+200];
          this.selectedImage_d2 = this.images_all[idx];
          this.selectedImage_d3 = this.images_all[idx+100];
          this.leftImage = 'dataset3';
          this.midImage = 'dataset1';
          this.rightImage = 'dataset2';
        }

        else {
          this.selectedImage_d1 = this.images_all[idx+100];
          this.selectedImage_d2 = this.images_all[idx+200];
          this.selectedImage_d3 = this.images_all[idx];
          this.leftImage = 'dataset2';
          this.midImage = 'dataset3';
          this.rightImage = 'dataset1';
        }
        this.radio_d1 = '';
        this.radio_d2 = '';
        this.radio_d3 = '';
      }
    }
  },

  computed: {
    isDisabled() {
      /** Button must be disabled when either the user has not ranked each image or 
       * ranked some of the images with the same number.
       */
      
      if (this.radio_d1 == '' || this.radio_d2 == '' || this.radio_d3 == ''){
        this.display_warning_ranking = 'block';
        this.button_state ='disabled';
        return true;
      }

      else if(this.radio_d1 !== this.radio_d2 && this.radio_d1 !== this.radio_d3
              && this.radio_d2 !== this.radio_d3) {
        this.display_warning_number = 'none';
        this.display_warning_ranking = 'none';
        this.button_state = 'enabled';
        return false;
      }

      else {
        this.display_warning_number = 'block';
        this.button_state = 'disabled';
        return true;
      }
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h1 {
  font-size: 2.5rem;
  color:#06ab92;
}

h3 {
  margin: 40px 0 0;
  font-size: 24px;
  font-style: bold;
  text-align: start;
  color:#06ab92;
}

.button {
    background-color: #06ab92 !important;
    color: white;
    font-size: 1.3rem;
    font-weight: bold;
    border-radius: 9px;
    margin: 2%;
    padding: 1%;
}

.button:hover {
  background-color: #06ab92;
  text-decoration: underline;
}

.content {
  color:#06ab92;
  text-align: center;
}

ul {
  list-style-type: none;
  padding: 0;
  display: grid;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #06ab92;;
}

p {
  padding-bottom: 4px;
  text-align: start;
}

sup {
  font-size: 12px;
}

.explanation {
  text-align: start;

}

.hello {
  margin-left: 3%;
  margin-right: 3%;
}

.image-container {
  display: flex;
  justify-content: center;
}

.dataset-1, .dataset-2, .dataset-3 {
  margin: 3%;
}

.user-id-input {
  border: 2px solid #06ab92;
  border-radius: 4px;
  margin-left: 30%;
  margin-right: 30%;
  margin-top: 2%;
  margin-bottom: 2%;
}

.user-id-input p {
  text-align: center;
  font-weight: 800;
}

.finished {
  margin: 0 auto;
  margin-top: 3%;
}

#warning-number, #warning-ranking {
  text-align: center;
  color: #800080;
  font-weight: bold;
}

/* Media Queries*/ 

@media (max-width: 550px) {
  
  h1 {
    font-size: 1.5rem;
  }

  h3 {
    font-size: 1.2rem;
  }
  
  .image-container {
    flex-wrap: wrap;
  }
}
</style>
