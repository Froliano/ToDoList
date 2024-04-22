function addTask() {
    var taskInput = document.getElementById('taskInput');
    var descriptionInput = document.getElementById('descriptionInput');
    var taskList = document.getElementById('taskList');
    var currentDate = new Date();

    if (taskInput.value.trim() !== '') {
        var li = document.createElement('li');
        li.innerHTML =  '<b class="title">Title : </b>' + 
                        '<span class="title"> ' + taskInput.value + '</span>' +
                        '<p>' + descriptionInput.value.replace(/\n/g, '<br>') + '</p>' +
                        '<div class="edit-buttons">' +
                        '<div id="slider">' +
                        '   <label class="switch">' +
                        '       <input type="checkbox" onchange="completeTask(this)">' +
                        '       <span class="slider"></span>' +
                        '   </label>'+
                        '   <p>Check when done</p>' +
                        '</div>'+
                        '<p> <b>Date de création : </b>'+ new Date(currentDate).toLocaleString() +' </p>' +
                        '<b>Date de modification : </b> <span id="dateModif"> '+ new Date(currentDate).toLocaleString() +' </span>' +
                        '<br>' + 
                        '  <button class="button" onclick="editTask(this)">Edit</button>' +
                        '  <button class="button" onclick="removeTask(this)">Remove</button>' +
                        '</div>';
        taskList.insertBefore(li, taskInput.firstChild);




        taskInput.value = '';
        descriptionInput.value = '';

        // Réorganiser la liste
        // reorderList();
        reorderList();
    }
}

function removeTask(button) {
    var taskList = document.getElementById('taskList');
    var task = button.parentNode.parentNode;
    // Suppression de la tâche de l'interface utilisateur
    taskList.removeChild(task);  
}

function completeTask(checkbox) {
    var taskText = checkbox.parentNode.parentNode.parentNode.parentNode;
    taskText.classList.toggle('completed');

    // Réorganiser la liste
    reorderList();
}

function modif(){
    var taskInput = document.getElementById('taskInput');
    var descriptionInput = document.getElementById('descriptionInput');
    var currentDate = new Date();

    var item = document.querySelector('.editing');
    item.querySelector("span").textContent = taskInput.value;
    item.querySelector('p').textContent = descriptionInput.value;
    item.querySelector("#dateModif").textContent = new Date(currentDate).toLocaleString();


    submitButton = document.getElementById('submit');
    modifButton = document.getElementById('modif');

    submitButton.style.display = 'block';
    modifButton.style.display = 'none';

    taskInput.value = "";
    descriptionInput.value = "";
    item.classList.remove('editing');
    if(!item.getElementsByTagName("input")[0].checked)
    {
        item.parentNode.insertBefore(item, item.parentNode.firstChild);
    }
    
}

function editTask(button) {
    window.scrollTo(0, 0);

    var item = button.parentNode.parentNode;
    var title = item.querySelector("span").textContent;
    var description = item.querySelector('p').textContent;

    document.getElementById('taskInput').value = title; 
    document.getElementById('descriptionInput').value = description;
    submitButton = document.getElementById('submit');
    modifButton = document.getElementById('modif');

    submitButton.style.display = 'none';
    modifButton.style.display = 'block';

    item.classList.add('editing');
}

function reorderList() {
    var taskList = document.getElementById('taskList');
    var items = Array.from(taskList.children);

    // Séparer les tâches complétées des tâches non complétées
    var completedTasks = items.filter(item => item.getElementsByTagName("input")[0].checked);
    var uncompletedTasks = items.filter(item => !item.getElementsByTagName("input")[0].checked);
    // Réorganiser la liste
    taskList.innerHTML = '';
    uncompletedTasks.forEach(task => taskList.appendChild(task));
    completedTasks.forEach(task => taskList.appendChild(task));
}   