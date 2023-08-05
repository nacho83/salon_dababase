#! /bin/bash
PSQL="psql -X --username=freecodecamp --dbname=salon --tuples-only -c"

echo -e "\n~~~~~ MY SALON ~~~~~\n"
echo -e "\nWelcome to My Salon, how can I help you?"
echo -e "\n"

MAIN_MENU() {
  
  LIST

  if [[ $1 ]]
  then
    echo -e "\n$1"
  fi

  read SERVICE_ID_SELECTED
  if [[ $SERVICE_ID_SELECTED > 5 ]]
  then
    echo "I could not find that service. What would you like today?"
    LIST
  else
      SERVICE_NAME=$($PSQL "SELECT name from services where service_id=$SERVICE_ID_SELECTED")
      echo $SERVICE_NAME
      echo "What's your phone number?"
      read CUSTOMER_PHONE
      DB_CUSTOMER_PHONE=$($PSQL "SELECT phone from customers where phone='$CUSTOMER_PHONE'")
      if [[ -z $DB_CUSTOMER_PHONE ]]
      then
        echo "I don't have a record for that phone number, what's your name?"
        read CUSTOMER_NAME
        INSERT_CUSTOMER=$($PSQL "INSERT INTO customers (phone,name) VALUES ('$CUSTOMER_PHONE','$CUSTOMER_NAME')")
        
        CUSTOMER_ID=$($PSQL "SELECT customer_id FROM customers where phone='$CUSTOMER_PHONE'")
        echo "What time would you like your $SERVICE_NAME, $CUSTOMER_NAME?"
        read SERVICE_TIME
        INSERT_APPOINTMENT=$($PSQL "INSERT INTO appointments(customer_id, service_id, time) VALUES ('$CUSTOMER_ID','$SERVICE_ID_SELECTED','$SERVICE_TIME')")
        echo "I have put you down for a $SERVICE_NAME at $SERVICE_TIME, $CUSTOMER_NAME."
      else
        CUSTOMER_ID=$($PSQL "SELECT customer_id FROM customers where phone='$CUSTOMER_PHONE'")
        echo "What time would you like your $SERVICE_NAME, $CUSTOMER_NAME?"
        read SERVICE_TIME
        INSERT_APPOINTMENT=$($PSQL "INSERT INTO appointments(customer_id, service_id, time) VALUES ('$CUSTOMER_ID','$SERVICE_ID_SELECTED','$SERVICE_TIME')")
        echo "I have put you down for a $SERVICE_NAME at $SERVICE_TIME, $CUSTOMER_NAME."
      fi
  fi


}

LIST(){
  AVAILABLE_SERVICES=$($PSQL "SELECT service_id, name FROM services") 
  echo "$AVAILABLE_SERVICES" | while read SERVICE_ID BAR NAME BAR
  do
    echo "$SERVICE_ID) $NAME"
   done
}

MAIN_MENU
