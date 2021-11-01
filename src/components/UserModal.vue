<template>
  <transition name="modal-fade">
    <div class="modal-backdrop">
      <div class="modal">
        <header class="modal-header">
          <slot name="header">
            User Details of {{ userData.name }}
          </slot>
          <button
            type="button"
            class="btn-close"
            @click="close"
          >
            x
          </button>
        </header>

        <section class="modal-body">
          <slot name="body">

            <table class="table table-striped table-bordered">
              <tbody>
                <tr>
                  <th>
                    ID
                  </th>
                  <td>
                    {{userData.id}}
                  </td>
                </tr>

                <tr>
                  <th>
                    Name
                  </th>
                  <td>
                    {{userData.name}}
                  </td>
                </tr>

                 <tr>
                  <th>
                    Username
                  </th>
                  <td>
                    {{userData.username}}
                  </td>
                </tr>

                 <tr>
                  <th>
                    Email
                  </th>
                  <td>
                    {{userData.email}}
                  </td>
                </tr>



                 <tr>
                  <th>
                    Address
                  </th>
                  <td>
                    {{userData.address.street}}, {{ userData.address.suite }}
                    <br>
                    {{ userData.address.city }} - {{ userData.address.zipcode }}
                  </td>
                </tr>



                 <tr>
                  <th>
                    Phone
                  </th>
                  <td>
                    {{userData.phone}}
                  </td>
                </tr>


                 <tr>
                  <th>
                    Website
                  </th>
                  <td>
                    {{userData.website}}
                  </td>
                </tr>


                 <tr>
                  <th>
                    Company
                  </th>
                  <td>
                    {{userData.company.name }}
                    <br>
                    <i>{{ userData.company.catchPhrase }}</i>
                   <br>
                  {{ userData.company.bs }}
                  </td>
                </tr>


              </tbody>
            </table>
          
          </slot>
        </section>

        <footer class="modal-footer">
          <slot name="footer">
            Return to User Profiles
          </slot>
          <button
            type="button"
            class="btn-green"
            @click="close"
          >
            Close Modal
          </button>
        </footer>
      </div>
    </div>
  </transition>  
</template>


<script>
  export default {
    name: 'Modal',
    props:['userData'],
    methods: {
      close() {
        this.$emit('close');
      },
    },
  };
</script>


<style>
  .modal-backdrop {
    position: fixed;
    top: 0;
    bottom: 0;
    left: 0;
    right: 0;
    background-color: rgba(0, 0, 0, 0.3);
    display: flex;
    justify-content: center;
    align-items: center;
  }

  .modal {
    background: #FFFFFF;
    box-shadow: 2px 2px 20px 1px;
    overflow-x: auto;
    display: flex;
    flex-direction: column;
  }

  .modal-header,
  .modal-footer {
    padding: 15px;
    display: flex;
  }

  .modal-header {
    position: relative;
    border-bottom: 1px solid #eeeeee;
    color: #4AAE9B;
    justify-content: space-between;
    font-size: 40px;
  }

  .modal-footer {
    border-top: 1px solid #eeeeee;
    flex-direction: column;
    justify-content: flex-end;
  }

  .modal-body {
    position: relative;
    padding: 20px 10px;
  }

  .btn-close {
    position: absolute;
    top: 0;
    right: 0;
    border: none;
    font-size: 20px;
    padding: 10px;
    cursor: pointer;
    font-weight: bold;
    color: #4AAE9B;
    background: transparent;
  }

  .btn-green {
    color: white;
    background: #4AAE9B;
    border: 1px solid #4AAE9B;
    border-radius: 2px;
  }

  .modal-fade-enter,
  .modal-fade-leave-to {
    opacity: 0;
  }

  .modal-fade-enter-active,
  .modal-fade-leave-active {
    transition: opacity .5s ease;
  }
</style>