
---

# 📚 TRW – Innovative Course Platform

TRW is a unique and creative course platform designed to inspire users to learn new skills and educate themselves in a modern and engaging way. This project started as a fun experiment but quickly grew into a serious web application combining Flask, HTML/CSS/JavaScript, and a dynamic user experience.

---

## 🚀 Project Background

When I was assigned a final project, I didn’t know what to build. So I decided to continue developing a platform I had started earlier — a course platform with three sections already in place.

The next challenge was Section 4: **interactive widgets in boxes** that would take the user to a new HTML page to view a **course intro video**. This was a completely new area for me and took a long time to get working. I also spent a huge amount of time on CSS (the file is now \~1600 lines), as I’m very particular about visual styling and layout.

After completing the widget system and linking each course to a background video, I built a **login and registration system** using Flask. During registration, users can choose which courses they’re interested in. Their choices are sent via a `POST` form and stored in the server using Flask and a database.

When a user logs in, their selected courses are retrieved and displayed on a personalized dashboard. From there, they can click on a course to access three related video lessons (currently implemented only for "Copywriting" and "Business" due to time constraints).

Finally, users can click the magical **"Log Out"** button, which redirects them to a stylish homepage.

---

## 🔧 How It Works

### Homepage (home.html)

Users land on the homepage where different courses are presented as **widgets**. Each widget is a clickable box:

```html
<div class="widgets">
    <a href="{{ url_for('video_page', video_name='copywriting') }}">
        <img src="..." alt="Copywriting Icon">
        <p>Copywriting</p>
    </a>
</div>
```

* The `video_name` attribute is passed into Flask to retrieve the course’s video info:

```python
videos = {
    "copywriting": {
        "title": "COPYWRITING",
        "filename": "Copywriting.mp4",
        "description": "Learn the secrets of persuasive writing to increase conversions."
    },
    ...
}
```

* Flask loads `video.html` where the video is automatically played:

```html
<video width="800" controls autoplay>
    <source src="{{ video_url }}" type="video/mp4">
</video>
<p>{{ video.description }}</p>
```

---

### Course Selection (Registration)

Each widget has a `data-course` attribute to help identify which course the user selects:

```html
<div class="widgets2" data-course="{{ slug }}">
    <img src="..." alt="{{ name }}">
    <p>{{ name }}</p>
</div>
```

JavaScript toggles a `selected` class when clicked:

```javascript
widgets.forEach(widget => {
    widget.addEventListener("click", function () {
        this.classList.toggle("selected");
    });
});
```

When registering, the form looks like this:

```html
<form action="{{ url_for('register') }}" method="post">
    <input type="text" name="username" required>
    <input type="password" name="password" required>
    <input type="hidden" id="selectedCourses" name="selectedCourses">
    <input type="submit" value="Register">
</form>
```

Before submission, JavaScript ensures at least one course is selected and stores the choices in a hidden input:

```javascript
selectedCoursesInput.value = selectedCourses.join(",");
```

On the server, Flask reads `request.form`, saves the data into the database, and redirects the user to the login page.

---

### Login System

```html
<form action="{{ url_for('login') }}" method="post">
    <input type="text" name="username" required>
    <input type="password" name="password" required>
    <input type="submit" value="Log in">
</form>
```

---

### User Dashboard

Once logged in, users see their chosen courses in a grid layout. Each course is clickable:

```html
<div class="course-grid">
    {% for course in selected_courses %}
        <div class="widget3" onclick="window.location.href='/course-videos?course={{ course }}'">
            <img src="..." alt="{{ course }}" class="course-icon">
            <p>{{ course.replace("-", " ")|title }}</p>
        </div>
    {% endfor %}
</div>
```

---

### Course Videos Page

Each course leads to a separate page with 3 video lessons (for some courses). Navigation is handled in JavaScript:

```javascript
const videos = {{ videos | tojson | safe }};
const videoData = videos[currentIndex];

videoElement.src = `https://.../${videoData.filename}`;
titleElement.textContent = videoData.title;
descriptionElement.textContent = videoData.description;
```

Users can click **Next** and **Previous** to cycle through the video content.

---

## 🛠️ Technologies Used

* **Flask (Python)** – backend framework
* **HTML/CSS** – frontend structure and design
* **JavaScript** – interactivity and dynamic rendering
* **Supabase** – media hosting
* **Jinja2** – template rendering

---

## ✅ Features

* Interactive homepage with course previews
* Dynamic widget selection and registration
* Login and user session handling
* Video-based learning system
* Personalized dashboard with selected content
* Responsive CSS design (\~1600 lines handcrafted)

---

## ⚠️ Notes

* Currently, full video content is available only for **Copywriting** and **Business**.
* The system can be scaled easily by adding more courses and videos.

---

Let me know if you’d like a markdown version (`README.md`) or help publishing this to GitHub Pages!
