<template>


  <div class="site-section my-pd" id="contact-section">
    <div class="container">
      <div class="row mb-5">
        <div class="col-12 text-center">
          <h2 class="section-title mb-3">Get In Touch</h2>
        </div>
      </div>
      <div class="row justify-content-center">
        <div class="col-lg-8 col-md-8 col-sm-12 mb-5">
          <form
            ref="form"
            class="p-5 bg-light rounded-0 position-relative overflow"
          >
          <input type="file" @change="loadFile($event)" multiple id="files" class="form-control rounded-0">

          </form>
        </div>
      </div>
    </div>
  </div>
</template>


<script>

export default {
  
  data(){
    return{

      lawn:{
        min_x:0,
        min_y:0,
        max_x:0,
        max_y:0,
      },
      
      mow:{
        position_x : 0,
        position_y : 0,
        direction : '',
      },
    }
  },
 
  methods:{

    // handle the file uploaded
    loadFile(event){
      let file = event.target.files[0]
      this.setData(file)
    },

    // set the data to the data object 
    setData(file){
        var reader = new FileReader();
        reader.onload = () => {

          let index = 0
          
          // get the data
          let data = reader.result.split('\n')

          // init the lawn Mow 
          this.initLawn(data[index].split(' '))
            
          // go to the next line
          index = index + 1
          // changing the mow 
          while (data[index]) {
            // int the mow 
            this.initMow(data[index].split(' '))
            // start programe
            this.startMow(data[index+1].split(''))

            alert(
              `a mow stops at cardinals : ${this.mow.position_x} ${this.mow.position_y} ${this.mow.direction}`
            )

 
            // // by pass to the next lines of the new mow
            index = index + 2

          }
        };

        reader.readAsText(file);
      
      },

      // init the lawn cardinals
      initLawn(cardinals){
        this.lawn.max_x = cardinals[0] * 1
        this.lawn.max_y = cardinals[1] * 1
      },

      // init the lawn cardinals
      initMow(cardinals){
        this.mow.position_x = cardinals[0] * 1
        this.mow.position_y = cardinals[1] * 1
        this.mow.direction = cardinals[2]
      },

      // start the Mow
      startMow(directions){
        directions.forEach(direction => {
          if(direction == 'L' || direction == 'R' ){
            this.changeMowSide(direction)
          }else if(direction == 'F'){
            this.moveMow()            
          }
        })
      },

      // change the side of mow
      changeMowSide(direction){

        if(direction == 'L'){
          
            if(this.mow.direction == 'N') this.mow.direction = 'W'
            else if(this.mow.direction == 'W') this.mow.direction = 'S'
            else if(this.mow.direction == 'S') this.mow.direction = 'E'
            else if(this.mow.direction == 'E') this.mow.direction = 'N'

            
        }else if(direction == 'R'){

            if(this.mow.direction == 'N') this.mow.direction = 'E'
            else if(this.mow.direction == 'E') this.mow.direction = 'S'
            else if(this.mow.direction == 'S') this.mow.direction = 'W'
            else if(this.mow.direction == 'W') this.mow.direction = 'N'

        }
      },

      // move the mow  or throw error (an simple alert fo that)
      moveMow(){

        if(this.mow.direction == 'N') this.mow.position_y + 1 > this.lawn.max_y ? alert('Out of lawn in N ') : this.mow.position_y = this.mow.position_y + 1 
        else if(this.mow.direction == 'S') this.mow.position_y - 1 < 0  ? alert('Out of lawn in S ') : this.mow.position_y = this.mow.position_y - 1 
        
        else if(this.mow.direction == 'E') this.mow.position_x + 1 > this.lawn.max_x ? alert('Out of lawn in E ') : this.mow.position_x = this.mow.position_x + 1 
        else if(this.mow.direction == 'W') this.mow.position_x - 1 < 0 ? alert('Out of lawn in W ') : this.mow.position_x = this.mow.position_x - 1 
        
      },
  }


}
</script>
 