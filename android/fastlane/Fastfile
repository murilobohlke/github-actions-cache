# This file contains the fastlane.tools configuration
# You can find the documentation at https://docs.fastlane.tools
#
# For a list of all available actions, check out
#
#     https://docs.fastlane.tools/actions
#
# For a list of all available plugins, check out
#
#     https://docs.fastlane.tools/plugins/available-plugins
#

# Uncomment the line if you want fastlane to automatically update itself
# update_fastlane

default_platform(:android)

platform :android do
  desc "Runs all the tests"
  lane :test do
    gradle(task: "test")
  end

  desc "Submit a new Beta Build to Play Store"
  lane :beta do
    store_password = prompt(text: "Signing Store Password: ", secure_text: true)
    key_password = prompt(text: "Alias Key Password: ", secure_text: true)
    releaseFilePath = File.join(Dir.pwd, "..", "my-release-key.keystore")
    gradle(task: 'clean')
    gradle(
      task: 'bundle',
      # task: 'assemble', # for apk
      build_type: 'Release',
      print_command: false,
      properties: {
        "android.injected.signing.store.file" => releaseFilePath,
        "android.injected.signing.store.password" => store_password,
        "android.injected.signing.key.alias" => "my-key-alias",
        "android.injected.signing.key.password" => key_password,
      }
    )
    upload_to_play_store(
      track: 'internal'
    )
  end

  desc 'Build and upload to App Center.'
  lane :appcenter do
    notes = prompt(text: "Release notes: ")

    appcenter_upload(
      api_token: "41b1b54ee4bc556d5d1256e9fbbdae65f61d1831",
      owner_name: "MuriloBohlke",
      app_name: "Water",
      apk: "./app/build/outputs/apk/release/app-release.apk",
      destinations: "*",
      notify_testers: true,
      release_notes: notes
    )
  end

  lane :playstore do
    gradle(
      task: 'assemble',
      build_type: 'Release'
    )
    upload_to_play_store(
      changes_not_sent_for_review: true
    )
  end
end
