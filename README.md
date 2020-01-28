How to Integrate TestRail with Pytest.

What is Pytest?

Pytest is a testing framework. This is a Python tool and used for all levels of automated software testing.


What is TestRail?

TestRail is a web-based test management tool used by testers, developers and other stakeholders to manage, track, organize software testing efforts.


Why Integration is Needed

When automated tests are executed we have to take results of each test case and mark them pass or fail. These results are taken into account for analysis purposes later.
Integration is required to define the result of test cases in TestRail. This depends on the result we get from automation scripts.

TestRail provides API to ‘integrate automated tests, submit results and other aspects”.

By calling TestRail API we can create Test Run and update results in TestRail. We can save time and effort of updating test cases status on TestRail after execution


Sample Test Code


    global client
    global run_id
    run_id = None
    client = APIClient('URL')
    client.user = 'UserID'
    client.password = 'Password'

    def __init__(self, driver):
        self.driver = driver

    def createTestRun(self, test_ids=[], name="Test Run Name"):
        response = client.send_post(
            'add_run/1',
            {'suite_id': 1,
             'name': name,
             'include_all': False,
             'case_ids': test_ids
             }
        )
        global run_id
        run_id = str(response["id"])

    def closeTestRun(self):
        global run_id
        response = client.send_post(
            'close_run/' + run_id,
            {}
        )

    def updateTestCase(self, testCaseId, result):
        global run_id
        if result == "pass":
            status = 1
        elif result == "fail":
            status = 5
        # else:
        # self.log.failed("Unknows Status ID for TestRail test update. Please use 'pass' or 'fail' for the tests.")
        print(testCaseId)
        print(run_id)
        response = client.send_post(
            'add_result_for_case/' + run_id + '/' + testCaseId,
            {
                'status_id': status,
                'comment': 'Test Executed - Status updated from Automated Smoke Test Suite'
            }
        )







Conclusion

To produce the results of automated test execution to a Test Management tool, the QA team earns time that can be spent on more testing. This will also ensure a good level of consistency across product and process deliverables. Please note that there may be more solutions to this, so feel free to choose the one that fits best for you or exercise a new one.
