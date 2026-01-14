from flask import Flask
from flask_sqlalchemy import SQLAlchemy
from flask_restful import Resource , Api , reqparse , fields , marshal_with , abort
# Some Initial Things and Configurations 
app = Flask(__name__)
app.config["SQLALCHEMY_DATABASE_URI"] = "sqlite:///api.db"
database = SQLAlchemy(app)
api = Api(app)


class UserModel(database.Model):
    id = database.Column(database.Integer , primary_key=True)
    name = database.Column(database.String(100) , unique=True , nullable=False)
    email = database.Column(database.String(100) , unique=True , nullable=False)
    def __repr__(self):
        return f"UserId : {self.id} , UserName : {self.name} , Email : {self.email}"

users_args = reqparse.RequestParser()
users_args.add_argument('name' , type=str , required=True , help="Name Cannot left blank")
users_args.add_argument('email' , type=str , required=True , help="Email Cannot left blank")


# TODO : Definition Of User Fields 
user_fields = {
    'id' : fields.Integer,
    'name' : fields.String,
    'email' : fields.String,
}


# TODO : Make API Endpoints
class Users(Resource):
    @marshal_with(user_fields)
    def get(self):
        users = UserModel.query.all()
        return users
    
    @marshal_with(user_fields)
    def post(self):
        args = users_args.parse_args()
        user = UserModel(name=args["name"],email=args["email"])
        database.session.add(user)
        database.session.commit()
        users = UserModel.query.all()
        return users , 201

class User(Resource):
    @marshal_with(user_fields)
    def get(slef , id):
        user = UserModel.query.filter_by(id=id).first()
        if not user :
            abort(404 , error_message="user not found")
        else:
            return user
    
    @marshal_with(user_fields)
    def delete(slef , id):
        user = UserModel.query.filter_by(id=id).first()
        if not user :
            abort(404 , error_message="user not found")
        
        database.session.delete(user)
        database.session.commit()
        users = UserModel.query.all()
        return users
api.add_resource(Users , '/api/users/')
api.add_resource(User , '/api/users/<int:id>')



if __name__ == "__main__":
    app.run(debug=True)