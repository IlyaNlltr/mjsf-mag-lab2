<template>
  <div class="todo">
    <div class="card text-left">
      <button type="button" class="btn btn-primary" data-toggle="modal" data-target="#exampleModal">
        Add task
      </button>

      <div class="modal fade" id="exampleModal" tabindex="-1" role="dialog" aria-labelledby="exampleModalLabel"
           aria-hidden="true">
        <div class="modal-dialog" role="document">
          <div class="modal-content">
            <div class="modal-header">
              <h5 class="modal-title" id="exampleModalLabel">Add task</h5>
              <button type="button" class="close" data-dismiss="modal" aria-label="Close">
                <span aria-hidden="true">&times;</span>
              </button>
            </div>
            <div class="modal-body">


              <form>
                <div class="form-group">

                  <label for="task-title">Task</label>

                  <input v-model="model.title" type="text" class="form-control" id="task-title"
                  >
                </div>
                <div class="form-group">
                  <label for="task-desc">Task description</label>
                  <textarea v-model="model.description" class="form-control" id="task-desc"></textarea>
                </div>

                <div class="dropdown">
                  <button class="btn" :class="{'btn btn-danger' :model.types==='Невідкладна',
                                               'btn btn-warning':model.types==='Важлива',
                                               'btn btn-success':model.types==='Звичайна',
                                               'btn btn-primary':model.types==='Незначна'}"
                          type="button"
                          data-toggle="dropdown" aria-haspopup="true" aria-expanded="false">
                    {{ model.types }}
                  </button>

                  <div class="dropdown-menu" aria-labelledby="dropdownMenuButton">
                    <button class="btn btn-danger" @click.prevent="model.types='Невідкладна'"  href="#">Невідкладна</button>
                    <br/>
                    <button class="btn btn-warning" @click.prevent="model.types='Важлива'"    href="#" >Важлива</button>
                    <br/>
                    <button class="btn btn-success" @click.prevent="model.types='Звичайна'"   href="#" >Звичайна</button>
                    <br/>
                    <button class="btn btn-primary" @click.prevent="model.types='Незначна'"   href="#" >Незначна</button>
                  </div>

                </div>

                <br/>
              </form>


            </div>
            <div class="modal-footer">
              <button type="button" class="btn btn-secondary" data-dismiss="modal">Close</button>
              <button type="button" class="btn btn-primary" @click.prevent="submit" :disabled="!isValid">Submit
              </button>
            </div>
          </div>
        </div>
      </div>

      <div class="card-header">
        ToDo Component
      </div>
      <div class="card-body">
        <h5 class="card-title">My tasks</h5>
        <div class="card-text">
          <ul class="pt-3">
            <li v-for="(item, index) in filteredTaskList" :key="index" class="d-flex justify-content-between mb-3">
              <div>
                <h4 v-if="item.status === 'completed'"><s>{{ item.title }}</s></h4>
                <h4 v-else>{{ item.title }}</h4>
                <p>{{ item.description }}</p>

                <div class="dropdown">
                  <button class="btn" :class="{'btn btn-danger':item.types==='Невідкладна',
                                               'btn btn-warning':item.types==='Важлива',
                                               'btn btn-success':item.types==='Звичайна',
                                               'btn btn-primary':item.types==='Незначна'}"
                          type="button"
                          data-toggle="dropdown" aria-haspopup="true" aria-expanded="false">
                    {{ item.types }}
                  </button>

                  <div class="dropdown-menu" aria-labelledby="dropdownMenuButton">
                    <button class="btn btn-danger" @click="item.types='Невідкладна'" href="#">Невідкладна</button>
                    <br/>
                    <button class="btn btn-warning" @click="item.types='Важлива'"    href="#">Важлива</button>
                    <br/>
                    <button class="btn btn-success" @click="item.types='Звичайна'"   href="#">Звичайна</button>
                    <br/>
                    <button class="btn btn-primary" @click="item.types='Незначна'"   href="#">Незначна</button>
                  </div>

                </div>
              </div>
              <button class="btn btn-primary" @click="item.status = 'completed' ">Compled</button>
            </li>
          </ul>
        </div>
        <div class="card-footer">
          <button class="btn" :class="{'btn-primary':!filterStatus}" @click="filterStatus = ''">All</button>
          <button class="btn" :class="{'btn-primary': filterStatus === 'completed'}"
                  @click="filterStatus = 'completed'">Completed
          </button>
          <button class="btn" :class="{'btn-primary': filterStatus === 'incompleted'}"
                  @click="filterStatus = 'incompleted'">InCompleted
          </button>
          Active tasks count: {{ taskList.length }}
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import {Task} from '../models/task';

export default {
  name: "ToDo",
  props: {},
  data: () => {
    return {
      model: new Task(),
      taskList: [],
      filterStatus: "",
    }
  },
  methods: {
    submit() {
      this.taskList.push(this.model);
      this.model = new Task();
    }
  },
  watch: {
    message() {
      console.log("Nessage changed");
    }
  },
  computed: {
    messageLength() {
      return ("" + this.message).length;
    },
    isValid() {
      return this.model.title && this.model.description && this.model.types;
    },
    filteredTaskList() {
      return this.taskList.filter((item) => {
        if (!this.filterStatus) return true;
        return item.status === this.filterStatus;
      });
    }
  }

}
</script>

<style scoped>
.todo {
  background-color: aqua;
}
</style>
