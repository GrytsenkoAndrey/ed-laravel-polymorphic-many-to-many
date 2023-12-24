# ed-laravel-polymorphic-many-to-many

## Step 1: Setting Up the Laravel Project

If you haven’t already, create a new Laravel project using the following command:

```
composer create-project --prefer-dist laravel/laravel many-to-many-polymorphic-relationship
```

Navigate to your project directory:

```
cd many-to-many-polymorphic-relationship
```

## Step 2: Database Configuration

Now, let’s set up a database and configure it in your .env file. Open the .env file and set the DB_DATABASE, DB_USERNAME, and DB_PASSWORD variables to match your database settings.

```
DB_CONNECTION=mysql
DB_HOST=127.0.0.1
DB_PORT=3306
DB_DATABASE=your_database_name
DB_USERNAME=your_database_username
DB_PASSWORD=your_database_password
```

After configuring your database, execute the migrations to create the necessary tables:

```
php artisan migrate
```

## Step 3: Creating Models

Next, we’ll create four models to represent a many-to-many polymorphic relationship. For this example, let’s work with a Tag model, an Image model, a Video model, and an Article model.

```
php artisan make:model Tag
php artisan make:model Image
php artisan make:model Video
php artisan make:model Article
```

## Step 4: Define the Relationship

In your Tag model, establish the many-to-many polymorphic relationship with the Image, Video, and Article models by creating a taggable method. This method uses the morphToMany Eloquent relationship:

```
// app/Tag.php
public function taggable()
{
    return $this->morphToMany([Image::class, Video::class, Article::class], 'taggable');
}
```

In your Image, Video, and Article models, define the inverse of the relationship by creating a tags method. This method uses the morphToMany Eloquent relationship as well:

```
// app/Image.php, app/Video.php, and app/Article.php
public function tags()
{
    return $this->morphToMany(Tag::class, 'taggable');
}
```

## Step 5: Creating and Retrieving Records

You can now create and retrieve records using the many-to-many polymorphic relationship. Here’s how you can associate tags with images, videos, and articles:

```
// Create tags
$tag1 = new Tag(['name' => 'Nature']);
$tag2 = new Tag(['name' => 'Technology']);
$tag1->save();
$tag2->save();

// Create an image
$image = new Image(['url' => 'https://example.com/image.jpg']);
$image->save();

// Associate tags with the image
$image->tags()->attach([$tag1->id, $tag2->id]);
```

To retrieve tags associated with an image:

```
$image = Image::find(1);
$tags = $image->tags;
```

Similarly, you can associate tags with videos and articles and retrieve them in the same way.

## Step 6: Updating and Deleting Records

Updating and deleting related records is straightforward. To update a tag’s name:

```
$tag = Tag::find(1);
$tag->update(['name' => 'Science']);
```

To remove a tag from an image, video, or article:

```
$image = Image::find(1);
$image->tags()->detach(1);
```
