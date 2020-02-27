from locust import HttpLocust, TaskSet, task, between
import logging

headers = {}


class UserBehavior(TaskSet):
    def on_start(self):
        """
            on_start is called when a Locust start before any task is scheduled
            used for loggin
        """
        self.login()

    def login(self):
        self.client.headers['Content-Type'] = "application/json"
        self.login = "trainerr"
        self.password = "password1"

        response = self.client.post(
            "/members/api/v1/sign_in",
            json={
                "type": "password",
                "login": self.login,
                "password": self.password
                })

        json_var = response.json()
        auth_token = json_var['session']['auth_token']
        headers['Auth-Token'] = auth_token

        logging.info('Login with login: %s and password: %s',
                     self.login, self.password)
        logging.info('Token: %s', auth_token)

    """
        Change based on needs
    """
    @task(1)
    def get_profile(self):
        self.client.get(
            "/members/api/v1/profile", headers=headers)


class WebsiteUser(HttpLocust):
    task_set = UserBehavior
    wait_time = between(5.0, 9.0)
