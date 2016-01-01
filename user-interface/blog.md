# ბლოგი

გაფართოება Blog საშუალებას იძლევა პუბლიკაციების შექმნის და კომენტარების საშუალებით მომხმარებლებთან ინტერაქტიული ურითიერთობის.

## პუბლიკაციები

ჩანართი **Posts**-ის საშუალებით იქმნება პოსტების შინაარსი . მენეჯერი საშუალებას იძლევა ახალი პოსტების შექმნის და უკვე არსებულის რედაქტირების.

თუ საჭიროა ერთ ან რამოდენიმე პოსტზე გარკვეული მოქმედების შესრულება, საჭიროა მონიშვნა პირველ სვეტში არსებული ბოქსის საშუალებით. ინსტრუმენტების ზოლი საშუალებას იძლევა პუბლიკაციის, გაუკმების, კოპირების და წაშლის ყველა მონიშნული პოსტისა.

როდესაც ემატება ან რედაქტირდება პოსტი, ხელმისაწვდომია შემდეგი ველები.

ველი           | აღწერა
:-------------- | :-----------------------------------------------------------------------------------------------------------------------------------
Title           | პოსტის სათაური.
Slug            | პოსტის ალტერნატივა. გამოიყენება პოსტის URL-ში (SEO-ს შესაბამისი).
Content         | Markdown ან HTML კონტენტი რომელსაც იყენებს [Pagekit editor](editor.md) რედაქტორი.
Excerpt         | შეიძლება შეიცავდეს პოსტის ამონარიდს.
Image           | პოსტთან ასოცირებული სურათი.
Status          | პოსტის სტატუსი.
Author          | პოსტის ავტორი.
Publish On      | პოსტის შექმნის თარიღი.
Restrict Access | პოსტზე დაშვების შეზღუდვა. შეუზღუდავია, თუ ველი ცარიელია
Enable Markdown | Markdown-ის დაშვება.
Enable Comments | პოსტზე კომენტარების დაშვება.

### პოსტის რედაქტირება

მეტი პოსტის რედაქტირებაზე მოცეცულია ბმულზე [Pagekit Editor](editor.md).

## კომენტარები
The **Comments** ჩანართი tab holds the comments manager for all the Blog posts.

## Settings
The **Settings** tab displays general Blog settings.

Field                 | Description
:-------------------- | :-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
**General**           |
Permalink             | The blog post link format. Allows choosing one or set a custom.
Posts per page        | Maximum amount of posts to be displayed per page.
Default post settings | Set the post defaults which can be overwritten for single posts.
**Comments**          |
Comments              | Require e-mail: if enabled users' email will be requested when they comment on a post. <br /> Close Comments: allows enabling and choosing the days after which the post comments will be disabled.
Appearance            | Show Avatars from Gravatar: when enabled the users email will be used to get and show their avatar picture from Gravatar service. <br /> Order comments by: allows choosing the comments display order. <br /> Enabled nested comments: allows choosing the maximum comments nested level.
