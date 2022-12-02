# AdooFinal

#markdown E-learning templates

    {% extends "base.html" %}
    {% load course %}
    {% block title %}
    Module {{ module.order|add:1 }}: {{ module.title }}
    {% endblock %}
    {% block content %}
    {% with course=module.course %}
    <h1>Course "{{ course.title }}"</h1>
    <div class="contents">
    <h3>Modules</h3>
    <ul id="modules">
    {% for m in course.modules.all %}
    <li data-id="{{ m.id }}" {% if m == module %}
    class="selected"{% endif %}>
    <a href="{% url "module_content_list" m.id %}">
    <span>
    Module <span class="order">{{ m.order|add:1 }}</span>
    </span>
    <br>
    {{ m.title }}
    </a>
    </li>
    {% empty %}
    <li>No modules yet.</li>
    {% endfor %}
    </ul>
    <p><a href="{% url "course_module_update" course.id %}">
    Edit modules</a></p>
    </div>
    <div class="module">
    <h2>Module {{ module.order|add:1 }}: {{ module.title }}</h2>
    <h3>Module contents:</h3>
    <div id="module-contents">
        {% for content in module.contents.all %}
        <div data-id="{{ content.id }}">
        {% with item=content.item %}
        <p>{{ item }}</p>
        <a href="{% url "module_content_update" module.id item|model_name item.id %}">Edit</a>
        
        <form action="{% url "module_content_delete" content.id %}"
        method="post">
        <input type="submit" value="Delete">
        {% csrf_token %}
        </form>
        {% endwith %}
        </div>
        {% empty %}
        <p>This module has no contents yet.</p>
        {% endfor %}
        </div>
        <h3>Add new content:</h3>
        <ul class="content-types">
        <li>
        <a href="{% url "module_content_create" module.id "text" %}">
        Text
        </a>
        </li>
        <li>
        <a href="{% url "module_content_create" module.id "image" %}">
        Image
