<!-- Custom error page -->

<template lang='pug'>

.error-page: template(v-if='!$fetchState.pending')

	//- Use blocks to render page
	blocks-list(v-if='page' :blocks='page.blocks')

	//- Or show simple message
	h1.style-h1(v-else) {{ message }}

</template>

<!-- ––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––– -->

<script lang='coffee'>

export default
	name: 'Error'


	props: ['error']

	data: ->
		page: null
		redirects: []

	fetch: ->

		# Get get the tower data


	# Show Sentry user input dialog if an error
	mounted: -> @showSentryDialog() if @uri == 'error'

	computed:

		# Figure out which error Tower to show
		uri: -> switch @error?.statusCode
			when 404 then 'page-not-found'
			else 'error'

		# Simple error message
		message: -> switch @error?.statusCode
			when 404 then 'Page Not Found'
			else 'An Error Occured'

	watch:

		# When redirects is set (which may be after mounted when SPAing)
		redirects:
			immediate: true
			handler: -> @redirectIfMatch()

	methods:

		# If the current path matches the redirect "from", then redirect
		redirectIfMatch: ->
			if match = @redirects.find ({ from }) => from == @$route.path
			then location.href = match.to

		# Show the sentry dialog
		# https://docs.sentry.io/enriching-error-data/user-feedback/?platform=browser
		showSentryDialog: -> @$defer => @$sentry?.showReportDialog()

</script>

<!-- ––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––– -->

<style lang='stylus' scoped>


</style>
