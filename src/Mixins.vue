<script>
export default {
  methods: {
    mediaQueryHandler (param, className) {
      const result = []
      param.split(/\s*(?:;|\s|,|$)\s*/).forEach((pair) => {
        const [key, value1, value2] = pair.split(':')
        if (key && value1 && value2) {
          result.push(`${key}:${className}-${value1}`)
          result.push(`${key}:${className}-${value2}`)
        } else if (key && value1) {
          result.push(`${key}:${className}-${value1}`)
        } else {
          result.push(`xs:${className}-${key}`)
        }
      })
      return result
    }
  }
}
</script>
