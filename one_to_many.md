# One To Many

In our example, we already have created a product and a review table (where products have many reviews), and are creating a user table. A user will have many products and reviews, but a product and a review can only have one user. So this is a one to many relationship.

When creating a user, you need to take the necessary steps to make sure that the relationship between the users and the other tables in your database are connected.

After creating your users migration, you need to add a references field into the tables that will relate to the users. At this point, your review table should already have a refernces feild for products. You don't need to add this into the users table because it is a one to many relationship.

Created a new migration for the necessary connections one at a time. In our example we would need to create:

    rails g migration AddUsersToProducts
    rails g migration AddUsersToReviews

Inside of the migration, it should have the following connection:

    def change
        add_reference :reviews, :user, foreign_key: true
    end

This is saying:

    add_reference <table_we're_changing> <table_we're_connection>, <making sure foreign_key is true>

Inside of the model for users (or the connecting table), we need to make sure to add the connections to the valid tables for active record. So in our case, user has many products and reviews. Inside of the users model, we would add:

    has_many :products, dependent: :nullify
    has_many :reviews, dependent: :nullify

The dependent variable accepts many different methods that affect how records are treated when users are deleted. If a user is deleted in this case, all the reviews and products belonging to the user are set to 'null' in their respective tables, but aren't removed. If we do `dependent: destroy`, it will delete them from their respective tables.

Similarly, we need to make sure that products and reviews are also connected so active record will have the correct methods for us to use. Product has_many reviews so it should also have similar connection like above. Reviews are a different case, because the reviews belong to both products and users, so in the reviews model we would need to add:

    belongs_to :product
    belongs_to :user
